# CiaoooNedos
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TrainEasy Jet - Votre Billet</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.4.0/jspdf.umd.min.js"></script>
    <script src="https://html2canvas.hertzen.com/dist/html2canvas.min.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-image: url('https://www.google.com/url?sa=i&url=https%3A%2F%2Ffr.vecteezy.com%2Fphotos-gratuite%2Ftrain&psig=AOvVaw3Ij7R3CwDP1rMAjrC7mGrg&ust=1754432098538000&source=images&cd=vfe&opi=89978449&ved=0CBIQjRxqFwoTCNjbpISX8o4DFQAAAAAdAAAAABAE');
            background-size: cover;
            background-position: center;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            flex-direction: column;
        }
        .ticket-container {
            display: flex;
            justify-content: center;
            width: 100%;
        }
        .ticket {
            background-color: rgba(255, 255, 255, 0.8);
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            width: 350px;
            padding: 20px;
            text-align: center;
            display: none;
        }
        .header {
            background-color: #005582;
            color: white;
            padding: 10px;
            border-radius: 5px;
            margin-bottom: 20px;
        }
        .popup {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            z-index: 100;
            text-align: center;
        }
        input {
            margin: 10px 0;
            padding: 8px;
            width: 100%;
            box-sizing: border-box;
        }
        button {
            background-color: #005582;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            margin: 5px;
        }
        button:hover {
            background-color: #003366;
        }
        .qr-code {
            margin: 20px auto;
        }
        .qr-code img {
            width: 100px;
            height: 100px;
        }
    </style>
</head>
<body>
    <div class="popup" id="popup">
        <h2>Petit malin, tu as essayé de cliquer pour obtenir ton billet !</h2>
        <p>Veuillez entrer votre prénom pour générer votre billet :</p>
        <input type="text" id="firstName" placeholder="Votre prénom">
        <button onclick="generateTicket()">Générer mon billet</button>
    </div>

    <div class="ticket-container">
        <div class="ticket" id="ticket">
            <div class="header">
                <h2>TrainEasy Jet</h2>
            </div>
            <h3>Billet Électronique</h3>
            <div class="ticket-info">
                <p><strong>Nom du Passager :</strong> <span id="passengerName">[Votre Nom]</span></p>
                <p><strong>Numéro de Train :</strong> VAN56000</p>
                <p><strong>Date de Départ :</strong> 17h44</p>
                <p><strong>Gare de Départ :</strong> Bistrot du Jardin</p>
                <p><strong>Gare d'Arrivée :</strong>
                <select id="arrivalStation" onchange="changeArrivalStation()">
                    <option value="Karaoké">Karaoké</option>
                    <option value="Bistrot du Jardin">Bistrot du Jardin</option>
                    <option value="Au Bureau">Au Bureau</option>
                    <option value="Au Lit">Au Lit</option>
                    <option value="Chez Joffray">Chez Joffray</option>
                </select></p>
                <p><strong>Numéro de Siège :</strong> <span id="seatNumber">[Numéro]</span></p>
                <p><strong>Voiture :</strong> <span id="carNumber">[Numéro]</span></p>
            </div>
            <div class="qr-code">
                <img src="https://quickchart.io/qr?text=https://www.google.com/maps/dir/Paris/Vannes/@47.9996772,-2.8291106,651494m/data=!3m2!1e3!4b1!4m14!4m13!1m5!1m1!1s0x47e66e1f06e2b70f:0x40b82c3688c9460!2m2!1d2.3513765!2d48.8575475!1m5!1m1!1s0x48101e84c37de291:0xb9f358307b233d13!2m2!1d-2.7599022!2d47.6586166!3e3?entry=ttu&g_ep=EgoyMDI1MDczMC4wIKXMDSoASAFQAw%3D%3D" alt="QR Code">
            </div>
        </div>
        <button onclick="generatePDF()" style="position: fixed; right: 20px; top: 20px;">Générer en PDF</button>
    </div>

    <script>
        function generateTicket() {
            const firstName = document.getElementById('firstName').value;
            if (firstName) {
                document.getElementById('passengerName').textContent = firstName;
                document.getElementById('popup').style.display = 'none';
                document.getElementById('ticket').style.display = 'block';
                changeNumbers();
            } else {
                alert('Veuillez entrer votre prénom.');
            }
        }

        function changeNumbers() {
            const seatNumber = Math.floor(Math.random() * 100) + 1;
            const carNumber = Math.floor(Math.random() * 20) + 1;
            document.getElementById('seatNumber').textContent = seatNumber;
            document.getElementById('carNumber').textContent = carNumber;
        }

        function changeArrivalStation() {
            const arrivalStation = document.getElementById('arrivalStation').value;
        }

        async function generatePDF() {
            const { jsPDF } = window.jspdf;
            const ticket = document.getElementById('ticket');
            const passengerName = document.getElementById('passengerName').textContent;

            const canvas = await html2canvas(ticket);
            const imgData = canvas.toDataURL('image/png');

            const pdf = new jsPDF();
            pdf.addImage(imgData, 'PNG', 10, 10, 180, 0);
            const message = `Merci ${passengerName} pour ces années à DBA !\nJ'en garderai un excellent souvenir !\nÀ très vite !`;
            const lines = pdf.splitTextToSize(message, 180);
            pdf.text(lines, 10, pdf.internal.pageSize.height - 30);
            pdf.save('billet_train.pdf');
        }
    </script>
</body>
</html>
