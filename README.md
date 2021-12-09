# :bulb: 당근클론

<img src="gitImages\Logo.png">

해당 강의는 코딩애플님의 <a href="https://codingapple.com/course/firebase-project/">당근마켓을 만들며 배워보는 Firebase</a> 를 통하여 제작되었음을 미리 밝힙니다.

# 📝 메인 페이지

<img src="gitImages\Index.png">

파이어베이스 DB에서 불러온 정보

```javascript
<script src="https://code.jquery.com/jquery-3.6.0.min.js" integrity="sha256-/xUj+3OJU5yExlq6GSYGSHk7tPXikynS7ogEvDej/m4=" crossorigin="anonymous"></script>
<script defer src="/__/firebase/9.6.0/firebase-app.js"></script>
<script defer src="/__/firebase/9.6.0/firebase-auth.js"></script>
<script defer src="/__/firebase/9.6.0/firebase-firestore.js"></script>
<script defer src="/__/firebase/9.6.0/firebase-storage.js"></script>
<script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/9.6.0/firebase-app.js";
    import { getAnalytics } from "https://www.gstatic.com/firebasejs/9.6.0/firebase-analytics.js";
    import { collection, getDocs, getFirestore } from 'https://www.gstatic.com/firebasejs/9.6.0/firebase-firestore.js'
    const firebaseConfig = {
    apiKey: "공개하지않음",
    authDomain: "공개하지않음",
    projectId: "공개하지않음",
    storageBucket: "공개하지않음",
    messagingSenderId: "공개하지않음",
    appId: "공개하지않음",
    measurementId: "공개하지않음"
    };
    const app = initializeApp(firebaseConfig);
    const analytics = getAnalytics(app);
    const db = getFirestore()
    const querySnapshot = await getDocs(collection(db, "product"));
    querySnapshot.forEach((doc) => {
    const htmlBox = document.createElement('div')
    htmlBox.className = 'product'
    htmlBox.innerHTML = `
        <div class="thumbnail" style="background-image: url('${doc.data().이미지||'https://via.placeholder.com/350'}')"></div>
        <div class="flex-grow-1 p-4">
        <h5 class="title">${doc.data().제목}</h5>
        <p class="date">2030년 1월 8일</p>
        <p class="price">${doc.data().가격}원</p>
        <p class="float-end">❤ 0 개</p>
        </div>
    `
    document.getElementsByClassName('container')[0].append(htmlBox) 
    });
</script>
```

를 바탕으로 forEach 반복문을 통해 모든 아이템을 조회하여 정보를 가져옴

# ✨ 업로드 페이지

<img src="gitImages\Upload.png">

파이어베이스 DB에

```javascript
const file = document.querySelector('#image').files[0]
const storageRef = storage.ref()
const storageURL = storageRef.child(`image/${file.name+new Date()}`)
const uploading = storageURL.put(file) 
uploading.on('state_changed', null, e => console.error(e), success => {
    uploading.snapshot.ref.getDownloadURL().then(url => {
    try {
        db.collection('product').add({
        가격: +price.value,
        내용: content.value,
        제목: title.value,
        날짜: new Date(),
        이미지: url
        }).then(() => window.location.href = '/index.html')
    } catch (e) {
        console.error("Error adding document: ", e);
    }
    })
})
```

해당 코드를 바탕으로 파일을 입력받아 정보를 넣어줌

# 🔥 로그인 페이지

<img src="gitImages\Login.png">

```javascript
firebase.initializeApp(firebaseConfig)
const db = firebase.firestore()
const storage = firebase.storage();
document.getElementById('register').addEventListener('click',() => {
    const 이메일 = document.getElementById('email-new').value
    const 패스워드 = document.getElementById('pw-new').value
firebase.auth().createUserWithEmailAndPassword(이메일,패스워드).then(res => console.log(res,res.user)).catch(e => console.error(e))
})
```

Authenticate를 호출하여 파이어베이스 인증 유저를 생성함