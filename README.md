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

        .button { /*버튼 관련 디자인*/
            display: inline-block;
            padding: 10px 20px;
            font-size: 16px;
            margin: 10px;
            text-decoration: none;
            color: #fff;
            background-color: #3498db;
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
    </style>
</head>
<body>

    <h1>백석대학교 분실물 웹 사이트</h1>
    <a href="#" class="button" id="findButton" onclick="showFindPage()">분실물 찾기</a> <!--분실물 찾기-->
    <a href="#" class="button" id="postButton" onclick="showPostPage()">분실물 등록</a> <!--분실물 등록-->
    <a href="#" class="button" id="boardButton" onclick="showBoardPage()">게시판</a> <!--게시판-->
    <div id="findPage" style="display: none;"><!--'분실물 찾기' 클릭시 나옴-->
        <h2>백석대학교 분실물 찾기</h2>
        <label for="searchInput">게시물 검색:</label><!--라벨태그-->
        <input type="text" id="searchInput" placeholder="게시물을 검색하세요"><!--인풋태그-->
        <button onclick="searchPosts()">검색</button><!--버튼태그-->
        <div id="searchResults"></div>
    </div>
    <div id="postPage" style="display: none;"><!--'분실물 등록' 클릭시 나옴-->
        <h2>백석대학교 분실물 등록</h2>
        <label for="postTitle">게시물 제목:</label><!--라벨태그-->
        <input type="text" id="postTitle" placeholder="게시물의 제목을 입력하세요"><!--인풋태그-->
        <div class="editor-container">
            <label for="postContent">게시물 내용:</label><!--라벨태그-->
            <textarea id="postContent" placeholder="게시물의 내용을 입력하세요"></textarea><!--텍스트애리어-->
        </div>
        <button onclick="submitPost()">등록</button><!--버튼태그-->
    </div>
    <div id="boardPage" style="display: none;"><!--'게시판' 클릭시 나옴-->
        <h2>백석대학교 분실물 게시판</h2>
        <div id="postList"></div>
    </div>

    <script>
        var posts = [];

        function showFindPage() {
            hideAllPages();
            document.getElementById('findPage').style.display = 'block';
        }

        function showPostPage() {
            hideAllPages();
            document.getElementById('postPage').style.display = 'block';
        }

        function showBoardPage() {
            hideAllPages();
            document.getElementById('boardPage').style.display = 'block';
            displayPosts();
        }

        function hideAllPages() {
            document.getElementById('findPage').style.display = 'none';
            document.getElementById('postPage').style.display = 'none';
            document.getElementById('boardPage').style.display = 'none';
        }

        function searchPosts() {
            var searchTerm = document.getElementById('searchInput').value.toLowerCase();
            var searchResults = posts.filter(function(post) {
                return post.title.toLowerCase().includes(searchTerm) || post.content.toLowerCase().includes(searchTerm);
            });

            displaySearchResults(searchResults);
        }

        function submitPost() {
            var title = document.getElementById('postTitle').value;
            var content = document.getElementById('postContent').value;
    
            posts.push({
                title: title,
                content: content
            });

            alert('게시물이 등록되었습니다:\n제목: ' + title + '\n내용: ' + content);
            showBoardPage();
        }

        function displayPosts() {
            var postListElement = document.getElementById('postList');
            postListElement.innerHTML = '';

            for (var i = 0; i < posts.length; i++) {
                var postElement = document.createElement('div');
                postElement.className = 'post';
                postElement.innerHTML = '<h3>' + posts[i].title + '</h3>' +
                                        '<p>' + posts[i].content + '</p>';
                postListElement.appendChild(postElement);
            }
        }

        function displaySearchResults(results) {
            var searchResultsElement = document.getElementById('searchResults');
            searchResultsElement.innerHTML = '';

            for (var i = 0; i < results.length; i++) {
                var resultElement = document.createElement('div');
                resultElement.className = 'post';
                resultElement.innerHTML = '<h3>' + results[i].title + '</h3>' +
                                          '<p>' + results[i].content + '</p>';
                searchResultsElement.appendChild(resultElement);
            }
        }
    </script>

</body>
</html>
