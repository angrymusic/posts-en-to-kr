# File vs Blob

원문: [https://jsdev.space/file-blob-js/](https://jsdev.space/file-blob-js/)

참고: [https://github.com/naver/fe-news/blob/master/issues/2024-11.md](https://github.com/naver/fe-news/blob/master/issues/2024-11.md)

# File과 Blob의 차이를 이해해보자

## 목차
#### [들어가며](#들어가며)
#### [1. Blob](#blob)
#### [2. File](#file)
#### [3. 사용 사례](#사용-사례)
#### [4. 요약](#요약)
#### [5. 나만의 예시](#나만의-예시)

## 들어가며

자바스크립트에서 File과 Blob 객체는 binary data를 처리하기 위해 사용되지만 이 둘은 각기 다른 목적과 성격을 가지고 있습니다. 그들의 차이점들에 대해 분석해봅시다!

## Blob

자바스크립트에서 **Blob** (Binary Large Object)는 binary data를 불변 객체로 표현하는 데이터 구조입니다. Blobs는 흔히 files, images, videos, 그리고 text가 아닌 data같은 raw binary data를 조작하고 처리하는데 사용됩니다.

### 주요 특징

1. 불변성 : 일단 생성되면, Blob은 수정될 수 없습니다. 여러분은 이미 존재하는 data를 조합해서 새로운 Blob을 생성할 수 있지만 Blob그 자체의 내용을 수정할 수 는 없습니다.
2. 데이터 표현: Blob은 text, images, audio, 또는 video를 포함한 어떤 타입의 data도 포함할 수 있습니다. 이를 통해 개발자들이 웹 어플리케이션에서 binary data를 다룰 수 있게 해줍니다.
3. 크기와 타입: Blob은 두 개의 주요 속성을 가집니다.
- **size**: 바이트 단위의 Blob 크기
- **type**: data의 MIME 타입  (예시: image/png, text/plain).
- **APIs**: Blob은 다양한 Web APIs에 사용됨
4. File API: 사용자가 업로드한 file들을 처리하는데 사용합니다.
- Canvas API: canvas 요소로부터 image들을 생성하는 데 사용합니다.
- Fetch API: binary data를 송수신 하는 데 사용합니다
5. Blob 생성하기: Blob 생성자를 통해 생성할 수 있습니다.

```jsx
const myBlob = new Blob(["Hello, world!"], { type: "text/plain" });
```

1. Object URL 생성하기:  **`URL.createObjectURL()`** 을 사용하여 Blob을 위한 임시 URL을 생성할 수 있습니다. 이를 통해 브라우저에서 Blob data에 접근할 수 있습니다.

```jsx
const url = URL.createObjectURL(myBlob);
const link = document.createElement("a");
link.href = url;
link.download = "hello.txt"; // Specify a default file name for download
link.textContent = "Download Blob";
document.body.appendChild(link);
```

1. Blob로 작업하기: `FileReader` API을 사용하여 Blob 내용을 읽을 수 있으며, `fetch` 나 `XMLHttpRequest` 을 통해 요청에 포함해 전송할 수도 있습니다.

### Blob 예시

```jsx
// Create a Blob
const myBlob = new Blob(["This is a sample text."], { type: "text/plain" });

// Create a URL for the Blob
const url = URL.createObjectURL(myBlob);

// Create a link to download the Blob
const downloadLink = document.createElement("a");
downloadLink.href = url;
downloadLink.download = "sample.txt"; // Specify the file name
downloadLink.textContent = "Download Sample Text";

// Append the link to the document
document.body.appendChild(downloadLink);
```

요약하자면 자바스크립트에서 Blob은 raw binary data를 다루기 위한 다재다능한 객체입니다. 이를 통해 업로드, 다운로드, 데이터 조작과 같은 기능을 웹 어플리케이션에서 구현할 수 있게 됩니다.

## File

File 객체는 사용자의 file system에 있는 파일을 나타냅니다. 파일의 이름, 크기, 타입, 그리고 최근 수정일등의 정보를 포함한 특정한 Blob의 타입입니다. File 객체는 주로 웹 어플리케이션에서 file 업로드나 사용자가 생성한 컨텐츠를 관리하는데 사용됩니다,

### 주요 특징

1. Blob으로 부터 상속:
File객체는 Blob 인터페이스를 확장하여 Blob의 속성, 메서드를 상속 받았습니다. 이를 통해 File 객체는 추가적인 정보를 제공하는 binary data의 blob처럼 취급됩니다.
2. 속성: File객체의 주요 속성들
- **name**: 확장자를 포함한 file의 이름
- **size**: byte 단위의 file 크기
- **type**: file의 MIME 타입 (예시 image/png, application/pdf)
- **lastModified**: 파일이 마지막으로 수정된 시간을 나타내는 timestamp (Unix epoch 이후 밀리초 단위)
- **webkitRelativePath**: 디렉토리 내에서 여러 파일을 선택할 때 주로 사용되는, 디렉토리에 대한 상대 경로
3. 생성:
File 객체는 일반적으로 사용자가 HTML input 요소(<input type=”file”>)로 file을 선택할 때 생성됩니다. 선택된 file(들)은 input 요소의 files 속성을 통해 접근할 수 있습니다.
4. FileReader API:
FileReader API는 종종 File 객체의 내용을 비동기적으로 읽을 때 사용됩니다. 이를 통해 file의 data를 text나 binary data로 읽을 수 있게 해주고 file의 내용을 표시하거나 처리할 수 있게 합니다.
5. API와의 사용:
File 객체는 `fetch` 나 `XMLHttpRequest` 같은 API들을 통해 서버로 보내질 수 있고 이를 통해 웹 애플리케이션에서 file을 업로드 하는데 필수적인 역할을 수행합니다.

### 사용 예시

```jsx
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>File Upload Example</title>
</head>
<body>

<input type="file" id="fileInput" />
<pre id="fileContent"></pre>

<script>
    const fileInput = document.getElementById('fileInput');
    const fileContent = document.getElementById('fileContent');

    fileInput.addEventListener('change', (event) => {
        const file = event.target.files[0]; // Get the first selected file

        if (file) {
            const reader = new FileReader();

            reader.onload = (e) => {
                fileContent.textContent = e.target.result; // Display the file content
            };

            reader.readAsText(file); // Read the file as text
        }
    });
</script>

</body>
</html>
```

요약하자면 자바스크립트에서 `File` 객체는 사용자 file system의 file을 표시하는데 좋은 도구입니다. file 조작에 필요한 속성들을 제공하고 웹 어플리케이션에서 흔히 file를 업로드하거나 처리할 때 사용됩니다. `FileReader API` 를 통해 `File` 객체를 활용함으로써 개발자들은 쉽게 사용자가 생성한 컨텐츠를 웹 환경에서 관리할 수 있게 됩니다.

## 사용 사례

### Blob

Blob은 흔히 data를 memory에서 관리하거나  `URL.createObjectURL()` 을 사용해 binary data의 URL을 만들거나 `fetch` 나 `XMLHttpRequest` 같은 API들을 통해 binary data를 전송할 때 사용됩니다.

### File

File객체는 주로 사용자가 업로드한 file을 다루거나 `FileReader` API를 사용하여 file을 읽거나 웹 어플리케이션에서 file과 관련된 API와 상호 작용할 때 사용됩니다.

### 예시

```jsx
// Create a Blob
const myBlob = new Blob(["Hello, world!"], { type: "text/plain" });

// Create a URL for the Blob
const blobUrl = URL.createObjectURL(myBlob);
console.log(blobUrl); // URL representation of the blob

// Handle File input
const fileInput = document.querySelector('input[type="file"]');
fileInput.addEventListener('change', (event) => {
  const file = event.target.files[0]; // Access the first selected file
  console.log(file.name); // Log the file name
  console.log(file.size); // Log the file size
});
```

## 요약

`File`과 `Blob` 객체들은 자바스크립트에서 binary data를 다룰 때 사용되고 File은 Blob의 file에 대한 추가적인 속성이 포함된 Blob의 확장된 타입입니다. 일반적으로 binary data를 사용할 때 Blob을 사용하고 사용자에 의해 선택되는 file을 작업하기 위해 File을 사용합니다.

## 나만의 예시

사용자가 올린 img를 메모리에서 바로 URL을 반환하여 업로드 전에 미리 보여주기

```jsx
const fileInput = document.querySelector('input[type="file"]');
fileInput.addEventListener('change', (event) => {
  const file = event.target.files[0];

  // File을 URL로 변환
  const fileUrl = URL.createObjectURL(file);

  // 변환된 URL을 img 태그의 src로 설정
  const imgElement = document.createElement('img');
  imgElement.src = fileUrl;
  document.body.appendChild(imgElement);
});

```
