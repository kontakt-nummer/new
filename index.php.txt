<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>What is my IP</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #f4f4f4;
        }
        .container {
            text-align: center;
        }
    </style>
</head>
<body>
    <div class="container">
        <h2 id="ipDetails">Your IPv4 Address</h2>
    </div>

    <script>
        window.onload = function() {
            getIPDetails();
        };

        function getIPDetails() {
            var xhr = new XMLHttpRequest();
            xhr.onreadystatechange = function() {
                if (this.readyState == 4 && this.status == 200) {
                    var ipAddress = JSON.parse(this.responseText);
                    document.getElementById("ipDetails").innerHTML = "<p><strong>IPv4 Address:</strong> " + ipAddress + "</p>";
                }
            };
            xhr.open("GET", "get_ipv4.php", true);
            xhr.send();
        }
    </script>

    <?php
    function getClientIP() {
        $ipAddress = '';

        if (!empty($_SERVER['HTTP_X_FORWARDED_FOR'])) {
            $ipAddress = $_SERVER['HTTP_X_FORWARDED_FOR'];
        } elseif (!empty($_SERVER['HTTP_CLIENT_IP'])) {
            $ipAddress = $_SERVER['HTTP_CLIENT_IP'];
        } elseif (!empty($_SERVER['REMOTE_ADDR'])) {
            $ipAddress = $_SERVER['REMOTE_ADDR'];
        } else {
            $ipAddress = 'UNKNOWN';
        }

        // Trim IPv6 address to remove any extra parts
        $ipAddress = preg_replace('/[^0-9a-fA-F:., ]/', '', $ipAddress);

        return $ipAddress;
    }

    echo getClientIP(); // To display the IP address directly
    ?>
</body>
</html>
