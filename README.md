# Control-web-page
Building a web page to control the direction of the robot’s movement and linking it to the database and creating a web page to display the stored data (last value)

* Install XAMPP and VS Code
* Create a file named robot1
  In the htdocs folder of XAMPP, create a new directory for your project
### 1. Create a Database:

* Open phpMyAdmin.
* Create a new database named "robot--control" for example.
* Create a table inside the database named "robot_movements"
```
CREATE TABLE robot_movements (
id INT AUTO_INCREMENT PRIMARY KEY,
direction VARCHAR(255) NOT NULL
);
``` 
   with the following columns:
  * id: A unique identifier for each movement (data type: INT).
  * direction: The direction of the robot's movement (data type: VARCHAR).
### 2.Control page
create a new file named index11.php and add the following code to create a simple control interface:
```
<!DOCTYPE html>
<html>
<head>
<title>Robot Control</title>
<style>
    .button-container {
        display: flex;
        justify-content: center;
        align-items: center;
        margin-top: 20px;
    }

    .button-container button {
        margin: 0 10px;
        width: 100px;
        height: 100px;
        font-size: 20px;
        background-color: #ff69b4;
        border-radius: 0;
    }
</style>
</head>
<body>
<h1 style="text-align: center;">Robot Control:<h1>
<form method="post" action="control11.php">
    <div class="button-container">
        <button name="direction" value="forward">Forward</button>
    </div>
    <div class="button-container">
        <button name="direction" value="left">Left</button>
        <button name="direction" value="stop">Stop</button>
        <button name="direction" value="right">Right</button>
    </div>
    <div class="button-container">
        <button name="direction" value="backward">Backward</button>
    </div>
</form>
</body>
</html>
```
### 3.The control script:
create a new file named control11.php in the control directoryand add the following code to process movement orders and update the database:

```
  <?php
$servername = "localhost";
$username = "root";
$password = "";
$dbname = "robot--control";

$conn = new mysqli($servername, $username, $password, $dbname);
if ($conn->connect_error) {
die("Connection failed: " . $conn->connect_error);
}

if (isset($_POST['direction'])) {
$direction = $_POST['direction'];

$sql = "INSERT INTO robot_movements (direction) VALUES ('$direction')";

if ($conn->query($sql) === TRUE) {
    echo "Robot movement recorded successfully.";
} else {
    echo "Error: " . $sql . "<br>" . $conn->error;
}
} else {
echo "No 'direction' value submitted in the POST request.";
}

$conn->close();
?>;

```
### 4.The presentation page:
create a new file named last value.php in the control directory and add the following code to retrieve the last recorded movement direction from the database and display it:
```
<!DOCTYPE html>
<html>
<head>
<title>Last Robot Movement</title>
<style>
  body {
      margin: 0;
      height: 100vh;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
  }
  .container {
      display: flex;
      flex-direction: column;
      align-items: center;
  }
  .movement-box {
      width: 100px;
      height: 100px;
      background-color: #ff69b4;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 24px;
      font-weight: bold;
      color: #333;
      border: 1px solid black;
      margin-bottom: 10px;
  }
  .back-button {
      background-color: #ff69b4;
      color: #333;
      text-decoration: none;
      padding: 10px 20px;
      border-radius: 0;
      font-size: 18px;
  }
</style>
</head>
<body>
  <div class="container">
    <h1>Last value:</h1>
    <div class="movement-box">
      <?php
      $servername = "localhost";
      $username = "root";
      $password = "";
      $dbname = "robot--control";

      $conn = new mysqli($servername, $username, $password, $dbname);

      if ($conn->connect_error) {
          die("Connection failed: " . $conn->connect_error);
      }

      $sql = "SELECT direction FROM robot_movements ORDER BY id DESC LIMIT 1";
      $result = $conn->query($sql);

      if ($result->num_rows > 0) {
          $row = $result->fetch_assoc();
          $lastDirection = $row["direction"];
          echo $lastDirection;
      } else {
          echo "No data";
      }

      $conn->close();
      ?>
    </div>
    <a href="index11.php" class="back-button">Go Back</a>
  </div>
</body>
</html>
```
## Web-based control interface:
![لوحة تحكم قاعدة البيانات](https://github.com/weaam-1424/Control-web-page/assets/173771361/26a3f0f4-5891-4279-a6df-37c2571f4bd2)

## Database interation:
![اختبار القاعده](https://github.com/weaam-1424/Control-web-page/assets/173771361/5ed7f03d-439e-4349-927e-8a672d8cf091)

## Last movement dirction:
![القيمه الاخيره](https://github.com/weaam-1424/Control-web-page/assets/173771361/dfe4c6fc-cecb-48c9-ad8c-91ba698bc230)


    

