# PMCLTrial
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Location Tracker</title>
</head>
<body>
    <h1>Location Tracker</h1>
    <button id="getLocation">Get Location</button>

    <script>
        document.getElementById('getLocation').addEventListener('click', getLocation);

        function getLocation() {
            if ('geolocation' in navigator) {
                navigator.geolocation.getCurrentPosition(handleSuccess, handleError);
            } else {
                alert('Geolocation is not supported by your browser.');
            }
        }

        function handleSuccess(position) {
            const latitude = position.coords.latitude;
            const longitude = position.coords.longitude;
            const timestamp = new Date().toLocaleString();

            // Replace 'YOUR_WEB_APP_URL' with the actual Web App URL obtained after deploying your Google Apps Script
            const scriptUrl = 'https://script.google.com/macros/s/AKfycbyZBctaADzZEZ_C1EcXSmd4eEaN7XQfSeaDOm3Ia5bRkpB2vYbDxPLGKQ9gMu8ykPO3/exec';
            const locationInfo = `Latitude: ${latitude}, Longitude: ${longitude}, Timestamp: ${timestamp}`;

            // Send the locationInfo to the server (Google Apps Script)
            sendLocationInfo(scriptUrl, locationInfo);
        }

        function handleError(error) {
            console.error('Error getting location:', error.message);
        }

        function sendLocationInfo(scriptUrl, locationInfo) {
            // Send the locationInfo to the server using an HTTP request (e.g., fetch)
            fetch(`${scriptUrl}?locationInfo=${encodeURIComponent(locationInfo)}`)
                .then(response => {
                    if (!response.ok) {
                        throw new Error('Network response was not ok');
                    }
                    return response.text();
                })
                .then(data => {
                    console.log('Location data sent successfully:', data);
                })
                .catch(error => {
                    console.error('Error sending location data:', error);
                });
        }
    </script>
</body>
</html>
