
# GitHub Workflows


## PHP CI

The PHP-CI workflow template provides an example to perform lint, phpcs, phpmd, phpstan and phpunit tasks with diffrent php and typo3 versions.  

Currently there are three jobs:
 *  PHP Lint  
    Depends on composer script `lint:php` and runs in diffrent php versions
 *  PHP QS  
    Depends on `test:phpcs`, `test:phpmd` and `test:phpstan` composer script and runs by default on php 7.4
 *  PHP Unit  
    Depends on composer script `test:phpunit` and runs in diffrent php versions for diffrend typo3 versions.


Example composer file with the dependencies and the required script section:
```json
{
    "require-dev": {
        "php-parallel-lint/php-parallel-lint": "^1.2",
        "sebastian/phpcpd": "^4.0 || ^5.0",
        "friendsofphp/php-cs-fixer": "^2.16",
        "phpmd/phpmd": "^2.9",
        "phpstan/phpstan": "^0.12.8",
        "phpstan/extension-installer": "^1.0",
        "saschaegerer/phpstan-typo3": "^0.13",
        "nimut/testing-framework": "^4.0 || ^5.0"
    },
    "config": {
        "vendor-dir": ".Build/vendor",
        "bin-dir": ".Build/bin"
    },
    "scripts": {
        "lint:php": [
            "[ -e .Build/bin/parallel-lint ] || composer update",
            ".Build/bin/parallel-lint ./Classes"
        ],
        "test:phpcs": [
            "[ -e .Build/bin/php-cs-fixer ] || composer update --ansi",
            ".Build/bin/php-cs-fixer fix -v --dry-run --diff  --ansi"
        ],
        "test:phpmd": [
            "[ -e .Build/bin/phpmd ] || composer update --ansi",
            ".Build/bin/phpmd ./Classes/ text phpmd.xml"
        ],
        "test:phpstan": [
            "[ -e .Build/bin/phpstan ] || composer update --ansi",
            ".Build/bin/phpstan analyse -c phpstan.neon --memory-limit=512M --ansi"
        ],
        "test:phpunit": [
            "[ -e .Build/bin/phpunit ] || composer update --ansi",
            "export TYPO3_PATH_WEB=$PWD/.Build/Web && .Build/bin/phpunit -c phpunit.xml.dist --colors=always"
        ]
    },
    "extra": {
        "typo3/cms": {
            "cms-package-dir": "{$vendor-dir}/typo3/cms",
            "web-dir": ".Build/Web"
        }
    }
}

```


## TYPO3 TER Release

This workflow template can be used to push an typo3 extension to the typo3 extension repository (ter).  
The release jobs runs automatically after a tag creation like `v10.4.12` (has to match `'v[0-9]+.[0-9]+.[0-9]+'`).


<!--
# Concluding
The Icons for the workflows are taken from https://typo3.github.io/TYPO3.Icons/
-->
