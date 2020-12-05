# code
<?php
// side effect: change ini setting
ini_set('erroe_reporting', E_All);

// side effect:; loads a file
include "file.php";

// side effect : generate output
echo "<html>\n";

// declartation
function foo()
{
//function body
}

//this is small example of PSR standard//

<?php
//declartion
function foo()
{
 //function body
 }
 
 conditional declartion is not a side effect
 if (! function_exist('bar')){
 
    //function body
    }
  }
  
  example of PSR
  public function my_method()
{
    // do something
}


public function myMethod()
{
    // do something
}
