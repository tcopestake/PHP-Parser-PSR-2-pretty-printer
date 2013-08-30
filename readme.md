# PHP-Parser PSR-2 pretty printer

##### Unsurprisingly, a PSR-2 pretty printer for [PHP-Parser](https://github.com/nikic/PHP-Parser).

## Installation

### Composer

Add the package to your composer.json requirements e.g.

    "require": {
        "tcopestake/php-parser-psr-2-pretty-printer": "dev-release"
    }

## Usage

To use this pretty printer, instantiate `PHPParserPSR2_Printer` instead of (for example) `PHPParser_PrettyPrinter_Default` and use it as you would normally. e.g:

    $printer = new PHPParserPSR2_Printer;

    ...

    echo $printer->prettyPrint($stmts);

## Features

Intentional changes that differ from the default pretty printer are:

* If more than one interface is implemented, each definition will go on a new line.
* Space between "use" and parameters in closure definitions.
* If more than two parameters are defined for a method, each definition goes on a new line.
* If more than two arguments are passed to a method, each argument goes on a new line.
* __Optional__ method renaming. (see below)

## Method renaming

By default, method names are considered volatile and are therefore not renamed. However, if you're feeling brave you can call `setMethodsVolatile(false)` to have the printer convert snake-cased method names to camel case. For example:

    protected function do_something() {
        return 1 + 1;
    }
    
    function get_class_to_do_something() {
        return $this->do_something();
    }

becomes:

    protected function doSomething()
    {
        return 1 + 1;
    }
    
    public function getClassToDoSomething()
    {
        return $this->doSomething();
    }

## Example

Input:

    class ABC extends \A implements \B, \C {
        public function a_method_name($awefwefwef, $ywefwefwefwefwef, $wwefwefwefwefwe) {
            $x = function() use($awefwefwef, $wwefwefwefwefwe)
            {
                if(1==1)
                    return $ywefwefwefwefwef;
            };

       // do something

            echo $x;
        }

        function bxy(array $g)
        {
                    // do something

            return $this->a_method_name($g, $f, $x);
        }

        function z(array $g)
        {
            foreach($y as $n)
            {

            }
        }
    }

Output:

    class ABC extends \A implements
        \B,
        \C
    {
        public function aMethodName(
            $awefwefwef,
            $ywefwefwefwefwef,
            $wwefwefwefwefwe
        ) {
            $x = function () use ($awefwefwef, $wwefwefwefwefwe) {
                if (1 == 1) {
                    return $ywefwefwefwefwef;
                }
            };
            // do something
            echo $x;
        }

        public function bxy(array $g)
        {
            // do something
            return $this->aMethodName(
                $g,
                $f,
                $x
            );
        }

        public function z(array $g)
        {
            foreach ($y as $n) {

            }
        }

    }

