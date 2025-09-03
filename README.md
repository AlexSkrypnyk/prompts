<p align="center"><img width="386" height="68" src="/art/logo.svg" alt="Laravel Prompts"></p>

<p align="center">
<a href="https://github.com/laravel/prompts/actions"><img src="https://github.com/laravel/prompts/workflows/tests/badge.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/prompts"><img src="https://img.shields.io/packagist/dt/laravel/prompts" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/prompts"><img src="https://img.shields.io/packagist/v/laravel/prompts" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/prompts"><img src="https://img.shields.io/packagist/l/laravel/prompts" alt="License"></a>
</p>

## About this fork

This is a fork of the original Laravel Prompts package, modified to support 
additional features and use cases. See [`ADDITIONAL_FEATURES.md`](ADDITIONAL_FEATURES.md)
for a list of changes and enhancements made in this fork.

I pledge to maintain this fork and keep it up to date with the latest changes 
from the original repository.

### Installation

Installed via Composer:

Add the following to your `composer.json` file under the `repositories` section:

```json
{
    "type": "vcs",
    "url": "https://github.com/AlexSkrypnyk/prompts.git",
    "canonical": true
}
```
Then run:

```bash
composer require laravel/prompts
```

### Versioning Policy

This fork follows the upstream project closely while providing additional
patches. To ensure compatibility with Composer and standard Semantic
Versioning (SemVer), releases in this fork are always published under the same
major and minor versions as upstream, with only the patch number incremented.

For example, if upstream’s latest release is `v0.3.6` and this fork introduces
extra fixes before upstream publishes a new version, the fork will tag its
release as `v0.3.7`. If upstream later publishes `v0.3.7`, the fork will move
forward and release `v0.3.8`. This way, patch numbers always increase
sequentially, avoiding conflicts with upstream tags and guaranteeing a clear
upgrade path.

Consumers requiring this fork simply add it as a VCS repository and keep using
the upstream package name. Composer will then resolve dependencies to the fork,
and applications using a constraint such as `^0.3` will transparently receive 
the patched releases without needing to change version requirements.

## Introduction

Laravel Prompts is a PHP package for adding beautiful and user-friendly forms to your command-line applications, with browser-like features including placeholder text and validation.

Laravel Prompts is perfect for accepting user input in your [Artisan console commands](https://laravel.com/docs/artisan#writing-commands), but it may also be used in any command-line PHP project.

## Official Documentation

Documentation for Laravel Prompts can be found on the [Laravel website](https://laravel.com/docs/prompts).

## Contributing

Thank you for considering contributing to Laravel Prompts! The contribution guide can be found in the [Laravel documentation](https://laravel.com/docs/contributions).

## Code of Conduct

In order to ensure that the Laravel community is welcoming to all, please review and abide by the [Code of Conduct](https://laravel.com/docs/contributions#code-of-conduct).

## Security Vulnerabilities

Please review [our security policy](https://github.com/laravel/prompts/security/policy) on how to report security vulnerabilities.

## License

Laravel Prompts is open-sourced software licensed under the [MIT license](LICENSE.md).
