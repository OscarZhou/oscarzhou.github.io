---
title: "PHP Regular Expression"
date: 2017-09-11
category: Other
tags: [Php, basic knowledge]
comments: true

---

## Introduction

In this article, I just show the basic use of some PHP regular expression, which is used for leading you to start PHP study.  
 
***

## Code 

    
    <?php
        echo "<p>Data processed</>";
        
        echo '<br/>';
        
        date_default_timezone_set('UTC');
        
        echo date('h:i:s:u a, l F jS Y e');
        
        /*
        // 获取表单传入数据
        $userName = $_POST['username'];
        
        echo $userName ."<br/>";
        */
        
        /*
            Integer, Float, String, Boolean, Array, Object
        */
        
        echo '<br/>';
        
        $userName = "Oscar";
        
        $str = <<<EOD
        The customers name is 
        $userName<br/>
    EOD;
        
        echo $str;
        
        echo '<br/>';
        
        define('PI', 3.1415926);
        echo "The value of PI is " . PI;
        
        echo '<br/>5 + 2 = ' . (5+2);
        echo '<br/>5 - 2 = ' . (5-2);
        echo '<br/>5 * 2 = ' . (5*2);
        echo '<br/>5 / 2 = ' . (integer)(5/2);
        echo '<br/>5 % 2 = ' . (5%2);
        
        echo '<br/>';
        $randNum = 5;
        echo "++randNum = " . ++$randNum . "</br>";
        echo "randNum++ = " . $randNum++ . "</br>";
        echo $randNum;
        
        echo '<br/>';
        $refToNum = &$randNum;
        $randNum = 100;
        echo '$refToNum = ' . $refToNum;
        
        echo '<br/>';
        
        if(5 == 10){
            echo '5 == 10';
        } else {
            echo '5 != 10';
        }
        
        echo '<br/>';
        
        $numOfOranges = 4;
        $numOfBananas = 36;
        if(($numOfOranges > 25) && ($numOfBananas > 30)){
            echo '25% Discount';
        } elseif(($numOfOranges > 30) || ($numOfBananas > 35)){
            echo '15% Discount';
        }elseif((!($numOfOranges < 5)) || (!($numOfBananas) < 5)){
            echo '5% Discount';
        }else {
            echo 'No discount for you';
        }
        
        echo '<br/>';
        
        $biggestNum = (15 > 10) ? 15 : 10;
        echo $biggestNum;
        
        
        echo '<br/>';
        
        switch($userName){
            case "Derek":
                echo "Hello Derek";
                break;
            
            case "Sally":
                echo "Hello Sally";
                break;
                
            default:
                echo "Hello Valued Customer";
                break;
                
        }
        
        echo '<br/>';
        
        
        $num = 0;
        while($num < 20){
            echo ++$num . ', ';
        }
        
        
        echo '<br/>';
        for($num = 1; $num <= 20; $num++){
            echo $num;
            if($num != 20){
                echo ', ';
                
            }else {
                break;
            }
        }
        
        
        echo '<br/>';
        
        $bestFriends = array('Joy', 'Willow', 'Ivy');
        
        echo 'My friend ' . $bestFriends[0];
        
        $bestFriends[4] = 'Steve';
        
        foreach($bestFriends as $friend){
            echo $friend . ',';
        }
        
        echo '<br/>';
        $streetAddress = "13 Pigeonwood Lane";
        $cityAddress = "Auckland";
        
        $customer = array('Name'=>$userName, 'Street' => $streetAddress, 'City'=>$cityAddress);
        
        foreach($customer as $key => $value){
            echo $key . ' : ' . $value . "<br/>";
        }
        
        echo '<br/>';
        
        $bestFriends = $bestFriends + $customer;
        
        echo '<br/>';
        
        $customers = array(array('Derek', '123 Main', '15212'),
                            array('Sally', '133 Main', '15212'),
                            array('Bob', '124 Main', '15212'));
                            
        for($row = 0; $row < 3; $row++){
            for($col = 0; $col < 3; $col++){
                echo $customers[$row][$col] . ', ';
            }
            echo '<br/>';
        }
        echo '<br/>';
        
        
        $randString = "              Random String          ";
        
        echo strlen("$randString") . "<br/>";
        echo strlen(ltrim("$randString")) . "<br/>";
        echo strlen(rtrim("$randString")) . "<br/>";
        echo strlen(trim("$randString")) . "<br/>";
        
        echo '<br/>';
        
        echo "The randString is $randString <br/>";
        
        printf("The randString is %s <br/>", $randString);
        
        echo '<br/>';
        
        $decimalNum = 2.3456;
        printf("decimal num = %.2f <br/>", $decimalNum);
        
        echo '<br/>';
        
        echo strtoupper($randString) . "<br/>";
        echo strtolower($randString) . "<br/>";
        echo ucfirst($randString) . "<br/>";
        
        $arrayForString = explode(' ', $randString, 2);
        $stringToArray = implode(' ', $arrayForString);
        
        $partOfString = substr("Random String", 0, 6);
        
        echo $partOfString;
        echo '<br/>';
        
        $man = "Man";
        $manhole = "Manhole";
        
        echo strcmp($man, $manhole);
        
        echo '<br/>';
        echo "The String " . strstr($randString, "String");
        echo '<br/>';
        echo "Loc The String " . strpos($randString, "String");
        echo '<br/>';
        $newString = str_replace("String", "Stuff", $randString);
        
        echo $newString . "<br/>";
        
        $dbString = '"Random quotes"';
        
        function addNumbers($num1, $num2){
            return $num1 + $num2;
        
        }
        
        echo "3 + 4 = " . addNumbers(3, 4);
    ?>
    
The result that corresponds the code above is shown below:
  
<p align="center">
<img src="/images/post/20170911006.png" alt="php regular expression" width="70%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>


***

It's quite interesting, Next plan is to apply these regular expression in web project. I am so excited.  