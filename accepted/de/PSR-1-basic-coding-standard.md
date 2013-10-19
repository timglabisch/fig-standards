Grundlegender Coding Standard
=====================

Dieser Abschnitt des Standards befasst sich damit, was sichergestellt sein muss,
um einen hohen Grad an technischer Interoperabilität
zwischen geteiltem PHP Code zu ermöglichen.


Die Schlüsselwörter "MUSS (MUST)", "DARF NICHT (MUST NOT)", "NOTWENDIG (REQUIRED)", "SOLLTE (SHALL)", "SOLLTE NICHT (SHALL NOT)", "SOLLTE (SHOULD)",
"SOLLTE NICHT (SHOULD NOT)", "IST EMPFOHLEN (RECOMMENDED)", "KANN (MAY)", and "OPTIONAL (OPTIONAL)" sind in [RFC 2119][] beschrieben.

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt
[PSR-0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md


1. Overview
-----------

- Dateien DÜRFEN NUR (MÜSSEN) die Tags `<?php` und `<?=` benutzen.

- Dateien DÜREN NUR (MÜSSEN) UTF-8 ohne BOM für PHP code nutzen.

- Files SHOULD *either* declare symbols (classes, functions, constants, etc.)
  *or* cause side-effects (e.g. generate output, change .ini settings, etc.)
  but SHOULD NOT do both.

- Dateien SOLLTEN *entweder* Symbole (classes, functions, constants, etc.) deklarieren
  *oder* Seiteneffekte erzeugen (generate output, change .ini settings, etc.)
  aber SOLLTEN NICHT für beides tun.

- Namespaces and Klassen MÜSSEN [PSR-0][] folgen.

- Klassennamen müssen in Form von `StudlyCaps` deklariert sein.

- Klassen Konstanten MÜSSEN in Versalien und mit Unterstrichen als Trenner geschrieben werden.

- Methodennamen Müssen in Form `camelCase` deklariert sein.


2. Dateien
--------

### 2.1. PHP Tags

In PHP Code MUSS die lange Schreibweise der PHP-Tags `<?php ?>` verwenden, alternativ für kurze Ausgaben `<?= ?>` ;
Es DÜRFEN KEINE anderen Tag-Variationen genutzt werden.

### 2.2. Zeichen Encoding

PHP Code DARF NUR (MÜSSEN) UTF-8 ohne BOM nutzen.

### 2.3. Seiteneffekte

Wenn eine Dateie neue Symbole (classes, functions, constants, etc.) deklariert,
so SOLLTE diese keine weiteren Seiteneffekte verursachen.
Alternativ SOLLTE die Datei Logik mit Seiteneffekten ausführen, aber sie SOLLTE NICHT beides tun.

Mit "Seiteneffekten" ist logik gemeint, welche nicht direkt damit verbunden ist Klassen, Fuktionen, Konstanten, ... zu deklarieren.
*abgesehen vom inkludieren der Datei*

"Seiteneffekte" beinhalten unter anderem: generieren von Output,
explizites nutzen von `require` or `include`, verbinden zu externen Services,
verändern von ini-Einstellungen, werfen von Fehlern oder Exceptions, verändern von globalen oder statischen variablen,
lesen oder schreiben von Dateien, und so weiter ...

The following is an example of a file with both declarations and side effects;
i.e, an example of what to avoid:

Das folgende Beispiel zeigt eine Datei mit Deklarationen und Seiteneffekten;
Achtung, Sollte vermieden werden:

```php
<?php
// Seiteneffekt: verändern von ini-Einstellungen
ini_set('error_reporting', E_ALL);

// Seiteneffekt: laden einer Datei
include "file.php";

// Seiteneffekt: generieren von Output
echo "<html>\n";

// Deklaration
function foo()
{
    // Funktions Body
}
```

The following example is of a file that contains declarations without side
effects; i.e., an example of what to emulate:

Das folgende Beispiel is stammt aus einer Datei welche Deklarationen ohne Seiteneffekte beinhaltet;
Positivbeispiel:

```php
<?php
// Deklaration
function foo()
{
    // Funktions Body
}

// Deklarationen welche abhängig einer Bedingung sind, sind *keine* Seiteneffekt
if (! function_exists('bar')) {
    function bar()
    {
        // function body
    }
}
```


3. Namespace and Class Names
----------------------------

Namespaces und Klassen MÜSSEN [PSR-0][] folgen.

Dies bedeutet jede Klasse entspricht einer Datei und ist in einem Namespace mit
mindestens einem Level: dem übergeordneten Anbieternamen

Klassennamen MÜSSEN als `StudlyCaps` deklariert sein.

Code welcher für PHP 5.3  oder neuer geschrieben wurde, MUSS Namespaces nutzen.

For example:

```php
<?php
// PHP 5.3 and later:
namespace Vendor\Model;

class Foo
{
}
```

Code welcher für PHP 5.2.x und älter geschrieben wurde, SOLLTE pseudo-namespacing Konventionen
von `Vendor_` Präfixen für Klassennamen verwenden.

```php
<?php
// PHP 5.2.x und früher:
class Vendor_Model_Foo
{
}
```

4. Class Konstanten, Properties, and Methoden
-------------------------------------------

Der Begriff "Klasse"  bezieht sich auf alle Klassen, Interfaces und Traits.

### 4.1. Konstanten

Klassen-Konstanten MÜSSEN in Versalien und mit unterstruchen als Trennzeichen geschrieben werden.
Beispiel:

```php
<?php
namespace Vendor\Model;

class Foo
{
    const VERSION = '1.0';
    const DATE_APPROVED = '2012-06-01';
}
```

### 4.2. Properties

Dieser Leitfahden beinhaltet absichtlich keine jede EMPFEHLUNG bezüglich der Verwendung von
`$StudlyCaps`, `$camelCase`, oder `$under_score` für Properties.

Whatever naming convention is used SHOULD be applied consistently within a
reasonable scope. That scope may be vendor-level, package-level, class-level,
or method-level.

Unabhängig von den Namenskonventionen SOLLTEN diese konsistent und vernünftig zum Anwendungsbereich passen.
Der Anwendungsbereich kann ahängig des Anbieter-Level, 'Package'-Level, Klassen-Level oder Methoden-Level sein.

### 4.3. Methods

Methodennamen MÜSSEN als `camelCase()` deklariert sein.
