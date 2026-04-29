<?php
$conn = new mysqli("localhost", "root", "", "love_db");

// Create tables automatically
$conn->query("CREATE TABLE IF NOT EXISTS posts (
    id INT AUTO_INCREMENT PRIMARY KEY,
    image VARCHAR(255),
    likes INT DEFAULT 0
)");

$conn->query("CREATE TABLE IF NOT EXISTS comments (
    id INT AUTO_INCREMENT PRIMARY KEY,
    post_id INT,
    comment TEXT
)");

// Upload Image
if(isset($_POST['upload'])) {
    $img = $_FILES['image']['name'];
    $tmp = $_FILES['image']['tmp_name'];
    move_uploaded_file($tmp, "uploads/".$img);
    $conn->query("INSERT INTO posts (image) VALUES ('$img')");
}

// Like
if(isset($_POST['like'])) {
    $id = $_POST['id'];
    $conn->query("UPDATE posts SET likes = likes + 1 WHERE id=$id");
}

// Comment
if(isset($_POST['comment_btn'])) {
    $id = $_POST['id'];
    $comment = $_POST['comment'];
    $conn->query("INSERT INTO comments (post_id, comment) VALUES ('$id','$comment')");
}
?>

<!DOCTYPE html>
<html>
<head>
<title>My Love 💖</title>
<style>
body {
    background: #ffe6f0;
    text-align: center;
    font-family: Arial;
}
h1 { color: red; }
.card {
    background: white;
    padding: 10px;
    margin: 10px;
    display: inline-block;
    border-radius: 10px;
}
</style>
</head>

<body>

<h1>❤️ My Love Gallery ❤️</h1>

<form method="POST" enctype="multipart/form-data">
    <input type="file" name="image" required>
    <button name="upload">Upload 💕</button>
</form>

<hr>

<?php
$result = $conn->query("SELECT * FROM posts");

while($row = $result->fetch_assoc()) {
?>
<div class="card">
    <img src="uploads/<?php echo $row['image']; ?>" width="200"><br><br>

    <form method="POST">
        <input type="hidden" name="id" value="<?php echo $row['id']; ?>">
        <button name="like
