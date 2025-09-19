<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>แชทออนไลน์ - ระบบห้องแชท</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .container {
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            overflow: hidden;
            width: 90%;
            max-width: 800px;
            min-height: 600px;
            display: flex;
            flex-direction: column;
        }

        .header {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            color: white;
            padding: 20px;
            text-align: center;
        }

        .header h1 {
            font-size: 24px;
            margin-bottom: 5px;
        }

        .status {
            font-size: 14px;
            opacity: 0.9;
        }

        .main-content {
            flex: 1;
            display: flex;
            flex-direction: column;
        }

        .lobby, .chat-room {
            flex: 1;
            padding: 30px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }

        .lobby {
            background: #f8f9ff;
        }

        .chat-room {
            background: #ffffff;
            justify-content: flex-start;
            padding: 20px;
        }

        .room-actions {
            display: flex;
            gap: 20px;
            margin-bottom: 30px;
        }

        .action-card {
            background: white;
            border-radius: 15px;
            padding: 30px;
            text-align: center;
            box-shadow: 0 10px 20px rgba(0,0,0,0.05);
            cursor: pointer;
            transition: all 0.3s ease;
            border: 2px solid transparent;
            min-width: 200px;
        }

        .action-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 30px rgba(0,0,0,0.1);
            border-color: #4facfe;
        }

        .action-card .icon {
            font-size: 48px;
            margin-bottom: 15px;
        }

        .action-card.create .icon {
            color: #4facfe;
        }

        .action-card.join .icon {
            color: #ff6b6b;
        }

        .action-card h3 {
            color: #333;
            margin-bottom: 10px;
            font-size: 18px;
        }

        .action-card p {
            color: #666;
            font-size: 14px;
        }

        .form-section {
            background: white;
            border-radius: 15px;
            padding: 30px;
            box-shadow: 0 10px 20px rgba(0,0,0,0.05);
            margin-top: 20px;
            width: 100%;
            max-width: 400px;
        }

        .form-group {
            margin-bottom: 20px;
        }

        .form-group label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #333;
        }

        .form-group input {
            width: 100%;
            padding: 12px 16px;
            border: 2px solid #e1e5e9;
            border-radius: 8px;
            font-size: 16px;
            transition: border-color 0.3s ease;
        }

        .form-group input:focus {
            outline: none;
            border-color: #4facfe;
        }

        .btn {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            color: white;
            border: none;
            padding: 14px 28px;
            border-radius: 8px;
            font-size: 16px;
            cursor: pointer;
            transition: all 0.3s ease;
            width: 100%;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(79, 172, 254, 0.3);
        }

        .btn-secondary {
            background: #6c757d;
            margin-top: 10px;
        }

        .btn-secondary:hover {
            background: #5a6268;
            box-shadow: 0 5px 15px rgba(108, 117, 125, 0.3);
        }

        .room-info {
            background: #e8f4fd;
            padding: 15px;
            border-radius: 10px;
            margin-bottom: 20px;
            border-left: 4px solid #4facfe;
        }

        .room-info h3 {
            color: #2c3e50;
            margin-bottom: 5px;
        }

        .room-code {
            font-family: 'Courier New', monospace;
            font-size: 18px;
            font-weight: bold;
            color: #4facfe;
            padding: 5px 10px;
            background: white;
            border-radius: 5px;
            display: inline-block;
            margin-top: 5px;
        }

        .messages {
            flex: 1;
            overflow-y: auto;
            padding: 20px;
            background: #f8f9fa;
            border-radius: 10px;
            margin-bottom: 20px;
            min-height: 300px;
            max-height: 400px;
        }

        .message {
            margin-bottom: 15px;
            padding: 10px 15px;
            border-radius: 18px;
            max-width: 70%;
            word-wrap: break-word;
            animation: fadeIn 0.3s ease;
        }

        .message.own {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            color: white;
            margin-left: auto;
            border-bottom-right-radius: 5px;
        }

        .message.other {
            background: white;
            border: 1px solid #e1e5e9;
            border-bottom-left-radius: 5px;
        }

        .message.system {
            background: #fff3cd;
            border: 1px solid #ffeaa7;
            text-align: center;
            margin: 10px auto;
            font-style: italic;
            color: #856404;
        }

        .message-input {
            display: flex;
            gap: 10px;
            padding: 0;
        }

        .message-input input {
            flex: 1;
            padding: 12px 16px;
            border: 2px solid #e1e5e9;
            border-radius: 25px;
            font-size: 16px;
        }

        .message-input input:focus {
            outline: none;
            border-color: #4facfe;
        }

        .send-btn {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 50%;
            cursor: pointer;
            transition: all 0.3s ease;
            width: 48px;
            height: 48px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .send-btn:hover {
            transform: scale(1.1);
        }

        .hidden {
            display: none;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .online-users {
            background: #e8f5e8;
            padding: 10px;
            border-radius: 8px;
            margin-bottom: 10px;
            font-size: 14px;
        }

        .user-count {
            color: #28a745;
            font-weight: bold;
        }

        @media (max-width: 768px) {
            .container {
                width: 95%;
                min-height: 90vh;
            }
            
            .room-actions {
                flex-direction: column;
                gap: 15px;
            }
            
            .action-card {
                min-width: auto;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>💬 แชทออนไลน์</h1>
            <div class="status">ระบบห้องแชทแบบ Real-time</div>
        </div>

        <div class="main-content">
            <!-- หน้าแรก -->
            <div id="lobby" class="lobby">
                <div class="room-actions">
                    <div class="action-card create" onclick="showCreateRoom()">
                        <div class="icon">🏠</div>
                        <h3>สร้างห้องแชท</h3>
                        <p>สร้างห้องแชทใหม่และเชิญเพื่อนมาร่วมสนทนา</p>
                    </div>
                    <div class="action-card join" onclick="showJoinRoom()">
                        <div class="icon">🚪</div>
                        <h3>เข้าร่วมห้องแชท</h3>
                        <p>ใช้รหัสห้องเพื่อเข้าร่วมการสนทนา</p>
                    </div>
                </div>

                <!-- ฟอร์มสร้างห้อง -->
                <div id="createRoomForm" class="form-section hidden">
                    <h3 style="margin-bottom: 20px; text-align: center; color: #333;">สร้างห้องแชทใหม่</h3>
                    <div class="form-group">
                        <label>ชื่อของคุณ</label>
                        <input type="text" id="createUsername" placeholder="ใส่ชื่อของคุณ">
                    </div>
                    <div class="form-group">
                        <label>ชื่อห้องแชท</label>
                        <input type="text" id="roomName" placeholder="ใส่ชื่อห้องแชท">
                    </div>
                    <button class="btn" onclick="createRoom()">สร้างห้องแชท</button>
                    <button class="btn btn-secondary" onclick="backToLobby()">ย้อนกลับ</button>
                </div>

                <!-- ฟอร์มเข้าร่วมห้อง -->
                <div id="joinRoomForm" class="form-section hidden">
                    <h3 style="margin-bottom: 20px; text-align: center; color: #333;">เข้าร่วมห้องแชท</h3>
                    <div class="form-group">
                        <label>ชื่อของคุณ</label>
                        <input type="text" id="joinUsername" placeholder="ใส่ชื่อของคุณ">
                    </div>
                    <div class="form-group">
                        <label>รหัสห้องแชท</label>
                        <input type="text" id="roomCode" placeholder="ใส่รหัสห้อง (6 หลัก)">
                    </div>
                    <button class="btn" onclick="joinRoom()">เข้าร่วมห้องแชท</button>
                    <button class="btn btn-secondary" onclick="backToLobby()">ย้อนกลับ</button>
                </div>
            </div>

            <!-- ห้องแชท -->
            <div id="chatRoom" class="chat-room hidden">
                <div class="room-info">
                    <h3 id="currentRoomName">ห้องแชท</h3>
                    <div>รหัสห้อง: <span class="room-code" id="currentRoomCode">------</span></div>
                    <div class="online-users">
                        👥 ผู้ใช้ออนไลน์: <span class="user-count" id="userCount">1</span> คน
                    </div>
                </div>

                <div id="messages" class="messages">
                    <div class="message system">ยินดีต้อนรับสู่ห้องแชท! 🎉</div>
                </div>

                <div class="message-input">
                    <input type="text" id="messageInput" placeholder="พิมพ์ข้อความ..." onkeypress="handleKeyPress(event)">
                    <button class="send-btn" onclick="sendMessage()">
                        <span style="font-size: 18px;">➤</span>
                    </button>
                </div>

                <button class="btn btn-secondary" onclick="leaveRoom()" style="margin-top: 20px;">ออกจากห้องแชท</button>
            </div>
        </div>
    </div>

    <script>
        // ระบบจำลอง WebSocket (ในการใช้งานจริงต้องใช้ WebSocket server)
        class ChatSystem {
            constructor() {
                this.rooms = new Map();
                this.currentUser = null;
                this.currentRoom = null;
                this.users = new Map();
            }

            generateRoomCode() {
                return Math.random().toString(36).substr(2, 6).toUpperCase();
            }

            createRoom(username, roomName) {
                const roomCode = this.generateRoomCode();
                const room = {
                    code: roomCode,
                    name: roomName,
                    users: new Set([username]),
                    messages: [],
                    created: new Date()
                };
                
                this.rooms.set(roomCode, room);
                this.currentUser = username;
                this.currentRoom = roomCode;
                
                return { success: true, roomCode, roomName };
            }

            joinRoom(username, roomCode) {
                const room = this.rooms.get(roomCode.toUpperCase());
                if (!room) {
                    return { success: false, error: 'ไม่พบห้องแชทนี้' };
                }

                room.users.add(username);
                this.currentUser = username;
                this.currentRoom = roomCode.toUpperCase();

                // เพิ่มข้อความระบบ
                room.messages.push({
                    type: 'system',
                    content: `${username} เข้าร่วมห้องแชท`,
                    timestamp: new Date()
                });

                return { success: true, roomName: room.name, messages: room.messages };
            }

            sendMessage(content) {
                if (!this.currentRoom || !this.currentUser) return false;

                const room = this.rooms.get(this.currentRoom);
                if (!room) return false;

                const message = {
                    type: 'user',
                    username: this.currentUser,
                    content: content,
                    timestamp: new Date()
                };

                room.messages.push(message);
                return message;
            }

            leaveRoom() {
                if (!this.currentRoom || !this.currentUser) return;

                const room = this.rooms.get(this.currentRoom);
                if (room) {
                    room.users.delete(this.currentUser);
                    
                    // เพิ่มข้อความระบบ
                    room.messages.push({
                        type: 'system',
                        content: `${this.currentUser} ออกจากห้องแชท`,
                        timestamp: new Date()
                    });

                    // ถ้าไม่มีใครในห้องแล้วลบห้อง
                    if (room.users.size === 0) {
                        this.rooms.delete(this.currentRoom);
                    }
                }

                this.currentUser = null;
                this.currentRoom = null;
            }

            getUserCount() {
                if (!this.currentRoom) return 0;
                const room = this.rooms.get(this.currentRoom);
                return room ? room.users.size : 0;
            }
        }

        // สร้างระบบแชท
        const chatSystem = new ChatSystem();

        // ฟังก์ชันสำหรับแสดงฟอร์ม
        function showCreateRoom() {
            document.getElementById('createRoomForm').classList.remove('hidden');
            document.querySelector('.room-actions').classList.add('hidden');
        }

        function showJoinRoom() {
            document.getElementById('joinRoomForm').classList.remove('hidden');
            document.querySelector('.room-actions').classList.add('hidden');
        }

        function backToLobby() {
            document.getElementById('createRoomForm').classList.add('hidden');
            document.getElementById('joinRoomForm').classList.add('hidden');
            document.querySelector('.room-actions').classList.remove('hidden');
        }

        // ฟังก์ชันสร้างห้อง
        function createRoom() {
            const username = document.getElementById('createUsername').value.trim();
            const roomName = document.getElementById('roomName').value.trim();

            if (!username || !roomName) {
                alert('กรุณากรอกข้อมูลให้ครบถ้วน');
                return;
            }

            const result = chatSystem.createRoom(username, roomName);
            if (result.success) {
                enterChatRoom(result.roomName, result.roomCode);
            }
        }

        // ฟังก์ชันเข้าร่วมห้อง
        function joinRoom() {
            const username = document.getElementById('joinUsername').value.trim();
            const roomCode = document.getElementById('roomCode').value.trim();

            if (!username || !roomCode) {
                alert('กรุณากรอกข้อมูลให้ครบถ้วน');
                return;
            }

            const result = chatSystem.joinRoom(username, roomCode);
            if (result.success) {
                enterChatRoom(result.roomName, roomCode.toUpperCase());
                // แสดงข้อความเก่า
                if (result.messages) {
                    result.messages.forEach(msg => {
                        if (msg.type !== 'system' || msg.content.includes(username)) {
                            displayMessage(msg);
                        }
                    });
                }
            } else {
                alert(result.error);
            }
        }

        // ฟังก์ชันเข้าสู่ห้องแชท
        function enterChatRoom(roomName, roomCode) {
            document.getElementById('lobby').classList.add('hidden');
            document.getElementById('chatRoom').classList.remove('hidden');
            document.getElementById('currentRoomName').textContent = roomName;
            document.getElementById('currentRoomCode').textContent = roomCode;
            updateUserCount();
            document.getElementById('messageInput').focus();
        }

        // ฟังก์ชันส่งข้อความ
        function sendMessage() {
            const input = document.getElementById('messageInput');
            const content = input.value.trim();

            if (!content) return;

            const message = chatSystem.sendMessage(content);
            if (message) {
                displayMessage(message);
                input.value = '';
                input.focus();
            }
        }

        // ฟังก์ชันแสดงข้อความ
        function displayMessage(message) {
            const messagesContainer = document.getElementById('messages');
            const messageElement = document.createElement('div');
            
            if (message.type === 'system') {
                messageElement.className = 'message system';
                messageElement.textContent = message.content;
            } else {
                const isOwn = message.username === chatSystem.currentUser;
                messageElement.className = `message ${isOwn ? 'own' : 'other'}`;
                
                if (!isOwn) {
                    const usernameSpan = document.createElement('div');
                    usernameSpan.style.fontSize = '12px';
                    usernameSpan.style.opacity = '0.7';
                    usernameSpan.style.marginBottom = '5px';
                    usernameSpan.textContent = message.username;
                    messageElement.appendChild(usernameSpan);
                }
                
                const contentDiv = document.createElement('div');
                contentDiv.textContent = message.content;
                messageElement.appendChild(contentDiv);
            }

            messagesContainer.appendChild(messageElement);
            messagesContainer.scrollTop = messagesContainer.scrollHeight;
        }

        // ฟังก์ชันออกจากห้อง
        function leaveRoom() {
            chatSystem.leaveRoom();
            document.getElementById('chatRoom').classList.add('hidden');
            document.getElementById('lobby').classList.remove('hidden');
            document.getElementById('messages').innerHTML = '<div class="message system">ยินดีต้อนรับสู่ห้องแชท! 🎉</div>';
            backToLobby();
            
            // ล้างฟอร์ม
            document.getElementById('createUsername').value = '';
            document.getElementById('roomName').value = '';
            document.getElementById('joinUsername').value = '';
            document.getElementById('roomCode').value = '';
        }

        // ฟังก์ชันอัพเดตจำนวนผู้ใช้
        function updateUserCount() {
            const count = chatSystem.getUserCount();
            document.getElementById('userCount').textContent = count;
        }

        // ฟังก์ชันจัดการการกด Enter
        function handleKeyPress(event) {
            if (event.key === 'Enter') {
                sendMessage();
            }
        }

        // จำลองการอัพเดตจำนวนผู้ใช้แบบ real-time
        setInterval(() => {
            if (chatSystem.currentRoom) {
                updateUserCount();
            }
        }, 5000);
    </script>
</body>
</html>
