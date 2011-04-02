This fork from leafo's lessphp is an attempt to bring PHP 5.3 conventions and
autoloading to the lessphp library so that it can be used easilly in a framework
like Symfony2.

# lessphp v0.2.0
#### <http://leafo.net/lessphp>

`lessphp` is a compiler for LESS written in php.
For a complete description of the language see <http://leafo.net/lessphp/docs/>

### How to use in your php project

Copy less.inc.php to your include directory and include it into your project.

There are a few ways to interface with the compiler. The easiest is to have it
compile a LESS file when the page is requested. The static function 
`Less::ccompile`, checked compile, will compile the input LESS file only when it
is newer than the output file.

    <?php
    require_once 'lessphp/src/autoload.php';
    use Lessphp\Lessc;
    
    try {
        Lessc::ccompile('input.less', 'output.css');
    catch (Exception $ex) {
        exit($ex->getMessage());
    }

Note that all failures with lessc are reported through exceptions.
If you need more control you can make your own instance of lessc.

    <?php
    require_once 'lessphp/src/autoload.php';
    use Lessphp\Lessc;

    $input = 'mystyle.less';

    $lc = new Lessc($input);

    try {
        file_put_contents('mystyle.css', $lc->parse());
    } catch (Exception $ex) { ... }

In addition to loading from file, you can also parse from a string like so:

    <?php
    require_once 'lessphp/src/autoload.php';
    use Lessphp\Lessc;

    $lc = new Lessc();
    $lesscode = 'body { ... }';
    $out = $lc->parse($lesscode);

### How to use from the command line

An additional script has been included to use the compiler from the command
line. In the simplest invocation, you specify an input file and the compiled
css is written to standard out:

    ~> plessc input.less > output.css

Using the -r flag, you can specify LESS code directly as an argument or, if 
the argument is left off, from standard in:

    ~> plessc -r "my less code here"

Finally, by using the -w flag you can watch a specified input file and have it 
compile as needed to the output file

    ~> plessc -w input-file output-file

Errors from watch mode are written to standard out.


