<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HealthSync Dashboard</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f3f4f6;
        }
    </style>
</head>

<body class="text-gray-800 h-screen flex overflow-hidden">

  <aside class="w-64 bg-white shadow-md hidden md:flex flex-col">
        <div class="p-6 flex items-center space-x-3 text-blue-600 font-bold text-2xl border-b border-gray-100">
            <i class="fas fa-heartbeat"></i>
            <span>HealthSync</span>
        </div>
        <nav class="flex-1 px-4 py-6 space-y-2">
            <a href="#" class="flex items-center space-x-3 px-4 py-3 bg-blue-50 text-blue-600 rounded-lg"><i
                    class="fas fa-home"></i><span>Dashboard</span></a>
            <a href="#" class="flex items-center space-x-3 px-4 py-3 text-gray-600 hover:bg-gray-50 rounded-lg"><i
                    class="fas fa-calendar-alt"></i><span>Appointments</span></a>
            <a href="#" class="flex items-center space-x-3 px-4 py-3 text-gray-600 hover:bg-gray-50 rounded-lg"><i
                    class="fas fa-envelope"></i><span>Messages</span></a>
            <a href="#" class="flex items-center space-x-3 px-4 py-3 text-gray-600 hover:bg-gray-50 rounded-lg"><i
                    class="fas fa-file-medical"></i><span>Records</span></a>
        </nav>
    </aside>

    <main class="flex-1 flex flex-col h-screen overflow-y-auto">
        <header class="bg-white shadow-sm px-8 py-4 flex justify-between items-center">
            <h1 class="text-2xl font-bold text-gray-800" id="welcome-text">Loading Dashboard...</h1>
            <div class="w-10 h-10 bg-blue-100 rounded-full flex items-center justify-center text-blue-600 font-bold">
                AJ
            </div>
        </header>

        <div id="dashboard-content" class="p-8">

            <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
                <div class="bg-white rounded-xl shadow-sm p-6 border border-gray-100">
                    <div class="flex justify-between items-center mb-4">
                        <h3 class="text-gray-500 font-medium">Heart Rate</h3>
                        <i class="fas fa-wave-square text-red-400"></i>
                    </div>
                    <p class="text-3xl font-bold text-gray-800" id="hr-val">--</p>
                </div>
                <div class="bg-white rounded-xl shadow-sm p-6 border border-gray-100">
                    <div class="flex justify-between items-center mb-4">
                        <h3 class="text-gray-500 font-medium">Blood Pressure</h3>
                        <i class="fas fa-tint text-blue-400"></i>
                    </div>
                    <p class="text-3xl font-bold text-gray-800" id="bp-val">--</p>
                </div>
                <div class="bg-white rounded-xl shadow-sm p-6 border border-gray-100">
                    <div class="flex justify-between items-center mb-4">
                        <h3 class="text-gray-500 font-medium">Weight</h3>
                        <i class="fas fa-weight text-green-400"></i>
                    </div>
                    <p class="text-3xl font-bold text-gray-800" id="weight-val">--</p>
                </div>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                <div class="bg-white rounded-xl shadow-sm border border-gray-100 p-6">
                    <h2 class="text-lg font-bold text-gray-800 mb-4 border-b pb-2">Upcoming Care</h2>
                    <div id="appointments-list" class="space-y-4">
                    </div>
                </div>

                <div class="bg-white rounded-xl shadow-sm border border-gray-100 p-6">
                    <h2 class="text-lg font-bold text-gray-800 mb-4 border-b pb-2">Recent Messages</h2>
                    <div id="messages-list" class="space-y-4">
                    </div>
                </div>
            </div>
        </div>
    </main>

    <script>
        // Mock Data hardcoded for instant hackathon demo
        const patientData = {
            user: { name: "Alex Johnson" },
            vitals: {
                heartRate: { value: 72, unit: "bpm" },
                bloodPressure: { value: "120/80", unit: "mmHg" },
                weight: { value: 165, unit: "lbs" }
            },
            appointments: [
                { id: 1, provider: "Dr. Sarah Smith", specialty: "Primary Care", date: "Tomorrow, 10:00 AM", status: "Upcoming" },
                { id: 2, provider: "Dr. James Lee", specialty: "Cardiology", date: "Nov 12, 2:30 PM", status: "Scheduled" }
            ],
            messages: [
                { id: 101, from: "Care Team", text: "Your lab results are ready to view.", time: "2 hours ago" }
            ]
        };

        // Populate the UI instantly
        document.addEventListener('DOMContentLoaded', () => {
            document.getElementById('welcome-text').innerText = `Welcome back, ${patientData.user.name.split(' ')[0]}`;

            document.getElementById('hr-val').innerText = `${patientData.vitals.heartRate.value} ${patientData.vitals.heartRate.unit}`;
            document.getElementById('bp-val').innerText = `${patientData.vitals.bloodPressure.value} ${patientData.vitals.bloodPressure.unit}`;
            document.getElementById('weight-val').innerText = `${patientData.vitals.weight.value} ${patientData.vitals.weight.unit}`;

            const apptContainer = document.getElementById('appointments-list');
            patientData.appointments.forEach(appt => {
                apptContainer.innerHTML += `
                    <div class="flex items-center justify-between p-4 bg-gray-50 rounded-lg">
                        <div>
                            <h4 class="font-bold text-gray-800">${appt.provider}</h4>
                            <p class="text-sm text-gray-500">${appt.specialty} • ${appt.date}</p>
                        </div>
                        <span class="px-3 py-1 bg-blue-100 text-blue-700 text-xs font-bold rounded-full">${appt.status}</span>
                    </div>
                `;
            });

            const msgContainer = document.getElementById('messages-list');
            patientData.messages.forEach(msg => {
                msgContainer.innerHTML += `
                    <div class="p-4 border-l-4 border-blue-500 bg-blue-50 rounded-r-lg">
                        <div class="flex justify-between items-center mb-1">
                            <h4 class="font-bold text-sm text-gray-800">${msg.from}</h4>
                            <span class="text-xs text-gray-500">${msg.time}</span>
                        </div>
                        <p class="text-sm text-gray-600">${msg.text}</p>
                    </div>
                `;
            });
        });
    </script>
</body>

</html>
