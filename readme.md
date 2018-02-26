
[![Latest Stable Version](https://poser.pugx.org/consilience/flysystem-azure-file-storage/v/stable)](https://packagist.org/packages/consilience/flysystem-azure-file-storage)
[![Total Downloads](https://poser.pugx.org/consilience/flysystem-azure-file-storage/downloads)](https://packagist.org/packages/consilience/flysystem-azure-file-storage)
[![Latest Unstable Version](https://poser.pugx.org/consilience/flysystem-azure-file-storage/v/unstable)](https://packagist.org/packages/consilience/flysystem-azure-file-storage)
[![License](https://poser.pugx.org/consilience/flysystem-azure-file-storage/license)](https://packagist.org/packages/consilience/flysystem-azure-file-storage)

# Azure File Storage adapater for Flysystem

This repo is fork of [League\Flysystem\Azure](https://github.com/thephpleague/flysystem-azure)

I separate service provider package for Laravel 5.5+ is available here:
https://github.com/academe/laravel-azure-file-storage-driver
The service provider allows Azure File Storage shares tbe be used
as native filesystems within Laravel.

# How to install

Install package
```bash
composer require consilience/flysystem-azure-file-storage
```

# How to use this driver

```php
use League\Flysystem\Filesystem;
use Consilience\Flysystem\Azure\AzureFileAdapter;
use MicrosoftAzure\Storage\File\FileRestProxy;
use Illuminate\Support\ServiceProvider;

// A helper method for constructing the connectionString will be implemented in time.

$connectionString = sprintf(
    'DefaultEndpointsProtocol=https;AccountName=%s;AccountKey=%s',
    '{storage account name}',
    '{file storage key}'
);

$config = [
    'endpoint' => $connectionString,
    'container' => '{file share name}',
    // Optional to prevent directory deletion recursively deleting
    // all descendant files and direcories.
    //'disableRecursiveDelete' => true,
];

$fileService = FileRestProxy::createFileService(
    $connectionString,,
    [] // $optionsWithMiddlewares
);


$filesystem = new Filesystem(new AzureFileAdapter(
    $fileService,
    $config,
    '' // Optional driver options.
));

// Now the $filesystem object can be used as a standard
// Flysystem file system.
// See https://flysystem.thephpleague.com/api/

// A few examples:

$content = $filesystem->read('path/to/my/file.txt');
$resource = $filesystem->readResource('path/to/my/file.txt');
$success = $filesystem->createDir('new/directory/here');
$success = $filesystem->rename('path/to/my/file.txt', 'some/other/folder/another.txt');
```

