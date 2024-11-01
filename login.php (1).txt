<?php 
include 'connect.php';

if(isset($_POST['signUp']))
{
	$username = $_POST['username'];
	$email= $_POST['email'];
	$password= $_POST['password'];
	$password=md5($password);
	$emailCheck= "SELECT * From users Where email='$email'";
	$result=$conn->query($emailCheck);
	
	$email = filter_var($email, FILTER_SANITIZE_EMAIL);
	if (!filter_var($email, FILTER_VALIDATE_EMAIL) === false) {
  echo("$email is a valid email address");
} else {
  echo("$email is not a valid email address");
}
	if($result->num_rows>0){
		echo "Email already exist.";
	}
	else {
		$insertQuery= "INSERT INTO users(username,email,password) VALUES ('$username', '$email', '$password')";

		if($conn->query($insertQuery) == TRUE){
			header("Location: index.html");
		}
		else {
		echo "Error: ".$conn->error;
		}
	}
}


if(isset($_POST['signIn']))
{
	$email= $_POST['email'];
	$password= $_POST['password'];
	$password=md5($password);
	
	$sql= "SELECT * From users Where email='$email' and password='$password'";
	$result =$conn->query($sql);
	
	if ($result->rowCount() > 0){
    while ($row = $result->fetch(PDO::FETCH_ASSOC))  
	if(isset($_POST['remember'])){
		setcookie('email', $email, time()+60*60*7);
		setcookie('pass', $password, time()+60*60*7);
	}
         session_start(); 
		$_SESSION['email']=$row['email'];
		header("Location: homepage.php");
    }
}
	else {
		echo "Not found, incorrect email or password.";
	}

?>