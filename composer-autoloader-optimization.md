# Composer Autoloader optimization

### Optimization Level 1: Class map generatio <a id="optimization-level-1-class-map-generation"></a>

There are a few options to enable this:

* Set `"optimize-autoloader": true` inside the config key of composer.json
* Call `install` or `update` with `-o` / `--optimize-autoloader`
* Call `dump-autoload` with `-o` / `--optimize`

### Optimization Level 2/A: Authoritative class maps <a id="optimization-level-2-a-authoritative-class-maps"></a>

There are a few options to enable this:

* Set `"classmap-authoritative": true` inside the config key of composer.json
* Call `install` or `update` with `-a` / `--classmap-authoritative`
* Call `dump-autoload` with `-a` / `--classmap-authoritative`

 **This option says that if something is not found in the classmap, then it does not exist and the autoloader should not attempt to look on the filesystem according to PSR-4 rules.** 

 **If your project or any of your dependencies does that then you might experience "class not found" issues in production. Enable this with care.** 

### Optimization Level 2/B: APCu cache <a id="optimization-level-2-b-apcu-cache"></a>

There are a few options to enable this:

* Set `"apcu-autoloader": true` inside the config key of composer.json
* Call `install` or `update` with `--apcu-autoloader`
* Call `dump-autoload` with `--apcu`

**Whether a class is found or not, that fact is always cached in APCu, so it can be returned quickly on the next request.** 

### Recommend: 

* Always enabled `opcache`
* Level 2/A



1. [https://getcomposer.org/doc/articles/autoloader-optimization.md](https://getcomposer.org/doc/articles/autoloader-optimization.md)

 

