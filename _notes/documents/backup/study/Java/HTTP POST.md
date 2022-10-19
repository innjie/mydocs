-   자원 요청 주소
-   `openConnection()` 이라는 메소드로 객체 연결

```
URL url = new URL("<https://www.google.com>");
HttpURLConnection connection = (HttpURLConnection) url.openConnection();

```

### 2. 요청 메소드 설정

-   requestMethod는 기본적으로 `GET`으로 설정되어 있으므로 사용할 메소드로 변경해야 한다.
-   GET 요청 시에는 요청 메소드를 GET으로 설정하고 OutputStream을 사용하지 않도록 작성

```
// get으로 설정
connection.setRequestMethod("GET");
// post로 설정
connection.setRequestMethod("POST");

```

### 3. 요청 헤더 설정

-   파라미터로 key, value를 받고 이름과 값을 설정한다.

```
connection.setRequestMethod("POST");

```

### 4. 데이터 넘기기

-   `POST`에서는 `OutputStream`으로 데이터를 전송
-   `setDoOutput()`으로 전송할 데이터가 있음을 전달
-   `checkConnected()` 메소드로 연결 상태 확인

```
DataOutputStream = new DataOutputStream(connection.getOutputStream());
outputStream.writeBytes(data);
outputStream.flush();
outputStream.close();

```

### 5. 응답 코드 / 데이터 얻기

-   응답 코드는 `HttpStatusCode`가 반환된다.

```
int responseCode = connection.getResponseCode();

```

-   응답 데이터는 `InputStream` 객체가 반환된다.
-   문자열 변환은 `BufferedReader`를 사용한다.

```
BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(connection.getInputStream()));
StringBuffer stringBuffer = new StringBuffer();
String inputLine;
while ((inputLine = bufferedReader.readLine()) != null) {
	stringBuffer.append(inputLine);
}
bufferedReader.close(); String response = stringBuffer.toString();

```