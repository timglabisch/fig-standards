Autoloading Standard
====================

Im folgenden werden die für eine Autoloader-Interoperabilität zwingenden
Anforderungen beschrieben.

Zwingend
--------

* Ein komplett ausgeschriebener Namespace mit Klasse muss die folgende
  Struktur einhalten `\<Anbieterbezeichnung>\(<Namespace>\)*<Klassenname>`
* Jeder Namespace muss auf höchster Ebene einen Namespace
  ("Anbieterbezeichnung") haben.
* Jeder Namespace kann beliebig viele Unter-Namespaces besitzen.
* Jeder Trenner für Namespaces wird beim Laden vom Dateisystem zu einem
  `DIRECTORY_SEPARATOR` konvertiert.
* Jedes `_` Zeichen im KLASSENNAMEN wird zu einem `DIRECTORY_SEPARATOR`
  konvertiert. Das Zeichen `_` hat keine besondere Bedeutung in einem
  Namespace.
* Der komplette Namespace und der Name der Klasse wird mit dem Suffix `.php`
  versehen, wenn diese vom Dateisystem geladen wird.
* Alphabetische Zeichen in Anbieterbezeichnungen, Namespaces und
  Klassennamen können in beliebiger Kombination aus Groß- und
  Kleinschreibung bestehen.

Beispiele
---------

* `\Doctrine\Common\IsolatedClassLoader` => `/path/to/project/lib/vendor/Doctrine/Common/IsolatedClassLoader.php`
* `\Symfony\Core\Request` => `/path/to/project/lib/vendor/Symfony/Core/Request.php`
* `\Zend\Acl` => `/path/to/project/lib/vendor/Zend/Acl.php`
* `\Zend\Mail\Message` => `/path/to/project/lib/vendor/Zend/Mail/Message.php`

Unterstriche in Namespaces und Klassennamen
-------------------------------------------

* `\namespace\package\Class_Name` => `/path/to/project/lib/vendor/namespace/package/Class/Name.php`
* `\namespace\package_name\Class_Name` => `/path/to/project/lib/vendor/namespace/package_name/Class/Name.php`

Der hier gesetzte Standard repräsentiert den kleinsten gemeinsamen Nenner
für schmerzfreie Autoloader-Interoperabilität. Zur Überprüfung, ob man
diesen Standards genügt, kann die nun folgende, beispielhafte Implementation
eines SplClassLoaders verwendet werden, welche Klassen ab PHP 5.3 laden kann.

Beispiel-Implementation
-----------------------

Es folgt eine Beispielfunktion, die demonstriert, wie den oben
vorgeschlagenen Standards entsprechende Klassen mit einem Autoloader
geladen werden können.

```php
<?php

function autoload($className)
{
    $className = ltrim($className, '\\');
    $fileName  = '';
    $namespace = '';
    if ($lastNsPos = strrpos($className, '\\')) {
        $namespace = substr($className, 0, $lastNsPos);
        $className = substr($className, $lastNsPos + 1);
        $fileName  = str_replace('\\', DIRECTORY_SEPARATOR, $namespace) . DIRECTORY_SEPARATOR;
    }
    $fileName .= str_replace('_', DIRECTORY_SEPARATOR, $className) . '.php';

    require $fileName;
}
```

SplClassLoader Implementation
-----------------------------

Das folgende Gist zeigt eine Beispiel-Implementation des
SplClassLoaders, mit deren Hilfe Klassen geladen werden können, wenn sie den
vorgeschlagenen Autoloader-Standards genügen. Es ist derzeit empfohlene
Methode, um in PHP 5.3 Klassen zu laden, die diesen Standards entsprechen.

* [http://gist.github.com/221634](http://gist.github.com/221634)

