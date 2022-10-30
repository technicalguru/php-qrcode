# technicalguru/qrcode
This is PHP implementation of QR Code 2-D barcode generator. It is pure-php
LGPL-licensed implementation based on C libqrencode by Kentaro Fukuchi.
This fork was restructured to work in a special namespace and with composer.

# LICENSE
This project is licensed under [GNU LGPL 3.0](LICENSE.md).

# INSTALLATION AND USAGE

## By Composer

```sh
composer require technicalguru/qrcode
```

## By Package Download
You can download the source code packages from [GitHub Release Page](https://github.com/technicalguru/php-qrcode/releases)

# SIMPLE EXAMPLE USAGE

```
$text = 'This is an example QR code';
$file = __DIR__.'/example-qr.png';

\QR\QRcode::png($text, $file, QR_ECLEVEL_L, 8);

echo 'QR code saved at '.$file;

```

# ADVANCED EXAMPLE USAGE

```
<?php
include 'vendor/autoload.php';

// set it to writable location, a place for temp generated PNG file
$PNG_TEMP_DIR = dirname(__FILE__).DIRECTORY_SEPARATOR.'qrcodes'.DIRECTORY_SEPARATOR;

// html PNG location prefix
$PNG_WEB_DIR = 'qrcodes/';

// of course we need rights to create temp dir
if (!file_exists($PNG_TEMP_DIR)) {
    mkdir($PNG_TEMP_DIR);
}

$filename = $PNG_TEMP_DIR.'test.png';

// processing form input
// remember to sanitize user input in real-life solution !!!
$errorCorrectionLevel = 'L';
if (isset($_REQUEST['level']) && in_array($_REQUEST['level'], array('L','M','Q','H'))) {
    $errorCorrectionLevel = $_REQUEST['level'];
}

$matrixPointSize = 4;
if (isset($_REQUEST['size'])) {
    $matrixPointSize = min(max((int)$_REQUEST['size'], 1), 10);
}

if (isset($_REQUEST['data'])) {

    // it's very important!
    if (trim($_REQUEST['data']) == '')
        die('data cannot be empty! <a href="?">back</a>');

    // user data
    $filename = $PNG_TEMP_DIR.'test'.md5($_REQUEST['data'].'|'.$errorCorrectionLevel.'|'.$matrixPointSize).'.png';
    \QR\QRcode::png($_REQUEST['data'], $filename, $errorCorrectionLevel, $matrixPointSize, 2);

} else {

    // default data
    echo 'You can provide data in GET parameter: <a href="?data=like_that">like that</a><hr/>';
    \QR\QRcode::png('PHP QR Code :)', $filename, $errorCorrectionLevel, $matrixPointSize, 2);

}

// display generated file
echo '<img src="'.$PNG_WEB_DIR.basename($filename).'" /><hr/>';

// config form
echo '<form method="post">
    Data:&nbsp;<input name="data" value="'.(isset($_REQUEST['data'])?htmlspecialchars($_REQUEST['data']):'PHP QR Code :)').'" />&nbsp;
    ECC:&nbsp;<select name="level">
        <option value="L"'.(($errorCorrectionLevel=='L')?' selected':'').'>L - smallest</option>
        <option value="M"'.(($errorCorrectionLevel=='M')?' selected':'').'>M</option>
        <option value="Q"'.(($errorCorrectionLevel=='Q')?' selected':'').'>Q</option>
        <option value="H"'.(($errorCorrectionLevel=='H')?' selected':'').'>H - best</option>
    </select>&nbsp;
    Size:&nbsp;<select name="size">';

for ($i=1; $i<=10; $i++) {
    echo '<option value="'.$i.'"'.(($matrixPointSize==$i)?' selected':'').'>'.$i.'</option>';
}

echo '</select>&nbsp;
    <input type="submit" value="GENERATE"></form><hr/>';

```

# CONTRIBUTION
Report a bug, request an enhancement or pull request at the [GitHub Issue Tracker](https://github.com/technicalguru/php-qrcode/issues).
Fell free to contact me via e-mail (deltalab at poczta dot fm) or using
folowing project pages:

# ACKNOWLEDGMENTS

Based on phpqrcode at SourceForge by Dominik Dzienia:

* [https://sourceforge.net/projects/phpqrcode/]()
* [https://phpqrcode.sourceforge.net/]()
  
Based on C libqrencode library (ver. 3.1.1) 
Copyright (C) 2006-2010 by Kentaro Fukuchi
http://megaui.net/fukuchi/works/qrencode/index.en.html

QR Code is registered trademarks of DENSO WAVE INCORPORATED in JAPAN and other
countries.

Reed-Solomon code encoder is written by Phil Karn, KA9Q.
Copyright (C) 2002, 2003, 2004, 2006 Phil Karn, KA9Q
 
