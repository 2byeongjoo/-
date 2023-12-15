
<!DOCTYPE html>
<html lang="ko">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>백석대학교 분실물 웹 사이트</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
        }

        header {
            background-color: #2350A9;
            color: white;
            padding: 10px;
            text-align: right;
            height: 30px;
            position: relative;
        }

        #headerImage {
            position: absolute;
            top: 50%;
            left: 0%;
            transform: translate(10%, -50%);
            margin: 0;
            width: 10%;
        }

        #welcomeMessage {
            font-size: 18px;
            position: absolute;
            top: 50%;
            right: 5%;
            transform: translate(-30%, -50%);
            margin: 0;
        }

        #loginBox {
            display: inline-block;
            margin-right: 20px;
        }

        #loginBox input {
            padding: 5px;
            margin-right: 5px;
        }

        #logout {
            position: absolute;
            top: 50%;
            right: 0%;
            transform: translate(-30%, -50%);
        }

        .button {
            display: inline-block;
            padding: 10px 20px;
            font-size: 16px;
            margin: 10px;
            text-decoration: none;
            color: #fff;
            background-color: #2350A9;
            border-radius: 5px;
            cursor: pointer;
        }

        .post {
            text-align: left;
            margin: 10px;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }

        .editor-container {
            text-align: left;
            margin: 10px;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }

        #postContent {
            width: 100%;
            height: 200px;
        }

        #imageInput {
            width: 100%;
            margin-top: 10px;
        }

        #commentsContainer {
            text-align: center;
            margin-top: 20px;
        }

        .comment {
            border: 1px solid #ddd;
            padding: 10px;
            margin-bottom: 10px;
            text-align: left;
            display: inline-block;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }

        th {
            background-color: #2350A9;
            color: #fff;
        }
    </style>
</head>

<body>
    <header id="mainHeader">

        <img id="headerImage" src="백석대.svg" alt="mainImage" />

        <div id="loginBox">
            <input type="text" id="username" placeholder="아이디">
            <input type="password" id="password" placeholder="비밀번호">
            <button onclick="login()">로그인</button>
        </div>
        <button onclick="logout()" id="logout" style="display: none;">로그아웃</button>
        <h1 id="welcomeMessage" style="display: none;"> 환영합니다, <span id="loggedInUsername"></span>님!</h1>

    </header>

    <h1>백석대학교 분실물 웹 사이트</h1>

    <a href="#" class="button" onclick="showLostItems()">습득물 리스트</a>
    <a href="#" class="button" onclick="showLostItemForm()">분실물 등록</a>
    <div id="lostItems" style="display: none;">
        <h2>습득물 리스트</h2>

        <table>
            <thead>
                <tr>
                    <th>ID</th>
                    <th>물품명</th>
                    <th>습득일</th>
                    <th>습득장소</th>
                </tr>
            </thead>
            <tbody id="lostItemsList"></tbody>
        </table>
    </div>
    <div id="lostItemForm"
        style="display: none; text-align: left; margin: 10px auto; padding: 10px; border: 1px solid #ccc; border-radius: 5px; width: 50%; text-align: center;">
        <h2>분실물 등록</h2>
        <form onsubmit="submitLostItem(); return false;" style="width: 80%; margin: 0 auto;">
            <label for="itemName">물품명:</label>
            <input type="text" id="itemName" required style="width: 50%;">
            <br>
            <label for="foundDate">습득일:</label>
            <input type="date" id="foundDate" required style="width: 50%;">
            <br>
            <label for="foundLocation">습득장소:</label>
            <input type="text" id="foundLocation" required style="width: 50%;">
            <br>
            <label for="imageInput">이미지 첨부:</label>
            <input type="file" id="imageInput" accept="image/*" style="width: 50%;">
            <br>
            <button type="submit" class="button" style="width: 50%;">등록</button>
        </form>
    </div>
    <div id="lostItemDetails" style="display: none; text-align: center;">
        <h2 id="itemDetailsTitle"></h2>
        <p id="itemDetailsFoundDate"></p>
        <p id="itemDetailsFoundLocation"></p>
        <img id="itemDetailsImage" style="max-width: 100%; margin: 10px auto; display: block;" alt="분실물 이미지">
        <div id="commentsContainer">
            <h3>댓글</h3>
            <form onsubmit="submitComment(); return false;">
                <input type="text" id="commentInput" p laceholder="댓글을 입력하세요" required>
                <button type="submit" class="button">등록</button>
            </form>
            <div id="commentsList"></div>
        </div>
        <a href="#" onclick="showLostItems()">돌아가기</a>
    </div>
</body>
<script>
    const users = [
        { username: "20204024", password: "0000", name: "홍해준" },
        { username: "20201930", password: "0000", name: "박승환" },
        { username: "20192799", password: "0000", name: "이병주" },
        { username: "20201643", password: "0000", name: "김진현" }
    ];
    login = () => {
        const enteredUsername = document.getElementById('username').value;
        const enteredPassword = document.getElementById('password').value;
        const user = users.find(u => u.username === enteredUsername && u.password === enteredPassword);

        if (user) {
            const mainHeader = document.getElementById('mainHeader');
            const welcomeMessage = document.getElementById('welcomeMessage');
            const loggedInUsername = document.getElementById('loggedInUsername');
            const logout = document.getElementById('logout');
            loggedInUsername.innerText = user.name;
            logout.style.display = 'block';
            welcomeMessage.style.display = 'block';
            document.getElementById('loginBox').style.display = 'none';

        } else {
            alert('아이디 또는 비밀번호가 올바르지 않습니다.');
        }
    }
    logout = () => {
        logoutMessage = confirm("로그아웃 하시겠습니까?");
        if(logoutMessage){
            alert("로그아웃 되었습니다.");
            document.getElementById('logout').style.display = 'none';
            document.getElementById('welcomeMessage').style.display = 'none';
            document.getElementById('loginBox').style.display = 'block';
            document.getElementById('username').value = null;
            document.getElementById('password').value = null;
        }else{
            alert("로그아웃을 취소하였습니다.");
        }
        
    }
    const lostItems = [];
    showLostItems = () => {
        hideAllSections();
        document.getElementById('lostItems').style.display = 'block';
        displayLostItems();
    }
    showLostItemForm = () => {
        hideAllSections();
        document.getElementById('lostItemForm').style.display = 'block';
    }

    showLostItemDetails = (itemId) => {
        hideAllSections();

        var item = lostItems.find(i => i.id === itemId);
        if (item) {
            document.getElementById('lostItemDetails').style.display = 'block';
            document.getElementById('itemDetailsTitle').innerText = item.name;
            document.getElementById('itemDetailsFoundDate').innerText = '습득일: ' + item.foundDate;
            document.getElementById('itemDetailsFoundLocation').innerText = '습득장소: ' + item.foundLocation;
            displayComments(item);
            displayItemImage(item.imageUrl);
        } else {
            showLostItems();
        }
    }
    displayItemImage = (imageUrl) => {
        var imageElement = document.getElementById('itemDetailsImage');
        if (imageUrl) {
            imageElement.src = imageUrl;
            imageElement.style.width = '600px';
            imageElement.style.display = 'block';
        } else {
            imageElement.style.display = 'none';
        }
    }
    submitPost = () => {
        var title = document.getElementById('postTitle').value;
        var content = document.getElementById('postContent').value;
        var imageInput = document.getElementById('imageInput');

        var imageUrl = null;
        if (imageInput.files.length > 0) {
            var imageFile = imageInput.files[0];
            var reader = new FileReader();
            reader.onload = function (e) {
                imageUrl = e.target.result;
                savePost(title, content, imageUrl);
            };
            reader.readAsDataURL(imageFile);
        } else {
            savePost(title, content, imageUrl);
        }
    }

    savePost = (title, content, imageUrl) => {
        posts.push({
            title: title,
            content: content,
            imageUrl: imageUrl
        });
        alert('게시물이 등록되었습니다:\n제목: ' + title + '\n내용: ' + content);
        showBoardPage();
    }
    hideAllSections = () => {
        document.getElementById('lostItems').style.display = 'none';
        document.getElementById('lostItemForm').style.display = 'none';
        document.getElementById('lostItemDetails').style.display = 'none';
    }
    displayLostItems = () => {
        var lostItemsListElement = document.getElementById('lostItemsList');
        lostItemsListElement.innerHTML = '';
        for (let i = 0; i < lostItems.length; i++) {
            var item = lostItems[i];
            var row = '<tr id="postlist" onclick="showLostItemDetails(' + item.id + ')"><td>' + item.id + '</td><td>' + item.name + '</td><td>' + item.foundDate + '</td><td>' + item.foundLocation + '</td></tr>';
            lostItemsListElement.innerHTML += row;
        }

    }

    submitLostItem = () => {
        var itemName = document.getElementById('itemName').value;
        var foundDate = document.getElementById('foundDate').value;
        var foundLocation = document.getElementById('foundLocation').value;
        var imageInput = document.getElementById('imageInput');
        if (imageInput.files.length === 0) {
            alert('이미지를 첨부해주세요.');
            return;
        }
        var newItem = {
            id: lostItems.length + 1,
            name: itemName,
            foundDate: foundDate,
            foundLocation: foundLocation,
            comments: [],
            imageUrl: null
        };
        var imageFile = imageInput.files[0];
        var reader = new FileReader();

        reader.onload = function (e) {
            newItem.imageUrl = e.target.result;
            saveLostItem(newItem);
            resetLostItemForm();
        };

        reader.readAsDataURL(imageFile);
    }
    resetLostItemForm = () => {
        document.getElementById('itemName').value = '';
        document.getElementById('foundDate').value = '';
        document.getElementById('foundLocation').value = '';
        document.getElementById('imageInput').value = '';
    }
    saveLostItem = (item) => {
        lostItems.push(item);
        alert('분실물이 등록되었습니다:\n물품명: ' + item.name + '\n습득일: ' + item.foundDate + '\n습득장소: ' + item.foundLocation);
        showLostItems();
    }
    submitComment = () => {
        var itemId = parseInt(document.getElementById('itemDetailsTitle').innerText);
        var commentInput = document.getElementById('commentInput').value;
        var item = lostItems.find(i => i.id === itemId);
        if (item) {

            item.comments.push(commentInput);
            displayComments(item);
            document.getElementById('commentInput').value = '';
        }
    }

    displayComments = (item) => {
        var commentsListElement = document.getElementById('commentsList');
        commentsListElement.innerHTML = '';
        for (var i = 0; i < item.comments.length; i++) {
            var comment = item.comments[i];
            var commentDiv = '<div class="comment">' + comment + '</div>';
            commentsListElement.innerHTML += commentDiv;
        }
    }
</script>
</body>

</html>
