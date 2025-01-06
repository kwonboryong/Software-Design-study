# CH2. 시스템 프로그래밍(파일 시스템 모듈)

- [파일 시스템 모듈(fs 모듈,FileSystem)](#파일-시스템-모듈fs-모듈filesystem)
- [fs 모듈](#fs-모듈)
- [glob 모듈](#glob-모듈)
- [fs-extra 모듈](#fs-extra-모듈)

<br/><br/>

---

# 파일 시스템 모듈(fs 모듈,FileSystem)
- Node.js 환경에서 사용할 수 있는 파일 관련 라이브러리
- **Node.js 내장 모듈**: `fs`, `path`, `os`, `stream` 등
    - 파일을 읽고 쓰거나, 경로를 다루는 작업을 할 때 사용
- **서드파티 라이브러리**: `glob`, `rimraf`, `mkdirp`, `fs-extra` 등
    - 파일 경로 검색이나 패턴 매칭 같은 고급 작업을 지원하는 라이브러리들
    - npm이나 pnpm을 통해 설치하여 사용할 수 있다.

<br/><br/>

---

# **`fs` 모듈**

: Node.js에서 파일/디렉터리를 직접 조작하거나 관리할 수 있는 **표준 라이브러리**

- **주요 기능**
    - 파일 읽기, 쓰기, 삭제
    - 디렉터리 생성, 삭제, 탐색
    - 파일 또는 디렉터리의 정보 확인
    - 동기 및 비동기 방식으로 파일/디렉터리 조작

```jsx
const fs = require('fs');

// 파일 읽기
fs.readFile('example.txt', 'utf8', (err, data) => {
  if (err) throw err;
  console.log(data);
});

// 디렉터리 생성
fs.mkdir('newDir', { recursive: true }, (err) => {
  if (err) throw err;
  console.log('디렉터리 생성 완료');
});
```

<br/><br/>

## `fs` 모듈 메서드
### **1. 파일 작업**
**`fs.readFile`** <br/>
: 파일의 내용을 비동기적으로 읽음
- **매개변수**
    1. `path` (string | Buffer | URL): 읽을 파일 경로
    2. `options` (string | object): 인코딩(예: `'utf8'`) 또는 플래그
    3. `callback` (function): 에러와 데이터를 처리하는 함수
```jsx
fs.readFile('example.txt', 'utf8', (err, data) => { ... });
```

<br/><br/>

**`fs.readFileSync`** <br/>
: 파일의 내용을 동기적으로 읽음
- **매개변수**: `path`, `options`
```jsx
const data = fs.readFileSync('example.txt', 'utf8');
```

<br/><br/>

**`fs.writeFile`** <br/>
: 파일에 데이터를 비동기적으로 씀
- **매개변수**
    1. `file` (string | Buffer | URL): 작성할 파일 경로
    2. `data` (string | Buffer | TypedArray): 파일에 쓸 데이터
    3. `options` (string | object): 인코딩 또는 플래그 설정
    4. `callback` (function): 에러를 처리하는 함수
```jsx
fs.writeFile('example.txt', 'Hello, Node.js!', (err) => { ... });
```

<br/><br/>

**`fs.writeFileSync`** <br/>
: 파일에 데이터를 동기적으로 씀
- **매개변수**: `file`, `data`, `options`
```jsx
fs.writeFileSync('example.txt', 'Hello, Node.js!');
```

<br/><br/>

**`fs.unlink`** <br/>
: 파일을 비동기적으로 삭제
- **매개변수**
    1. `path` (string | Buffer | URL): 삭제할 파일 경로
    2. `callback` (function): 에러를 처리하는 함수
```jsx
fs.unlink('example.txt', (err) => { ... });
```

<br/><br/>

**`fs.unlinkSync`** <br/>
: 파일을 동기적으로 삭제
- **매개변수**: `path`
```jsx
fs.unlinkSync('example.txt');
```

<br/><br/><br/>

### **2. 디렉터리 작업** 
**`fs.readdir`** <br/>
: 디렉터리 내용을 비동기적으로 읽음
- **매개변수**
    1. `path` (string | Buffer | URL): 디렉터리 경로
    2. `options` (string | object): 인코딩 설정
    3. `callback` (function): 에러와 파일 목록을 처리하는 함수
```jsx
fs.readdir('./', (err, files) => { ... });
```

<br/><br/>

**`fs.readdirSync`** <br/>
: 디렉터리 내용을 동기적으로 읽음
- **매개변수**: `path`, `options`
```jsx
const files = fs.readdirSync('./');
```

<br/><br/>

**`fs.mkdir`** <br/>
: 디렉터리를 비동기적으로 생성
- **매개변수**
    1. `path` (string | Buffer | URL): 생성할 디렉터리 경로
    2. `options` (object)
        - `recursive`: 중첩 디렉터리 생성 여부 (기본값: `false`)
    3. `callback` (function): 에러를 처리하는 함수
```jsx
fs.mkdir('new_directory', { recursive: true }, (err) => { ... });
```

<br/><br/>

**`fs.mkdirSync`** <br/>
: 디렉터리를 동기적으로 생성
- **매개변수**: `path`, `options`
```jsx
fs.mkdirSync('new_directory', { recursive: true });
```

<br/><br/>

**`fs.rmdir`** *(Node.js 14 이하에서 사용, 최신 버전에서는 `fs.rm` 권장)* <br/>
: 디렉터리를 비동기적으로 삭제
- **매개변수**
    1. `path` (string | Buffer | URL): 삭제할 디렉터리 경로
    2. `options` (object)
        - `recursive`: 하위 디렉터리 포함 삭제 여부 (기본값: `false`)
    3. `callback` (function): 에러를 처리하는 함수
```jsx
fs.rmdir('new_directory', { recursive: true }, (err) => { ... });
```

<br/><br/>

**`fs.rmdirSync`** <br/>
: 디렉터리를 동기적으로 삭제
- **매개변수**: `path`, `options`
```jsx
fs.rmdirSync('new_directory', { recursive: true });
```

<br/><br/><br/>

### **3. 파일 정보**
**`fs.stat`** <br/>
: 파일이나 디렉터리의 정보를 비동기적으로 가져옴
- **매개변수**
    1. `path` (string | Buffer | URL): 확인할 경로
    2. `callback` (function): 에러와 정보를 처리하는 함수
```jsx
fs.stat('example.txt', (err, stats) => { ... });
```

<br/><br/>

**`fs.statSync`** <br/>
: 파일이나 디렉터리의 정보를 동기적으로 가져옴
- **매개변수**: `path`
```jsx
const stats = fs.statSync('example.txt');
```

<br/><br/><br/>

### **4. JSON 파일 작업**
**JSON 읽기** <br/>
: JSON 파일의 데이터를 읽고 파싱
```jsx
fs.readFile('data.json', 'utf8', (err, data) => {
  const jsonData = JSON.parse(data);
});
```

<br/><br/>

**JSON 쓰기** <br/>
: 객체를 JSON 형식으로 파일에 저장
```jsx
const jsonData = { key: 'value' };
fs.writeFile('data.json', JSON.stringify(jsonData, null, 2), (err) => { ... });
```

<br/><br/>
<br/>

---

# **`glob` 모듈**
: 파일 경로를 패턴(와일드카드)을 사용해 검색할 수 있도록 제공하는 **서드파티 라이브러리**
- **주요 기능**
    - 특정 경로에 있는 파일들을 와일드카드 패턴으로 필터링하여 검색
    - 복잡한 파일 탐색을 간단하게 처리
    - Node.js에서 파일 매칭 작업을 위해 주로 사용
```jsx
const glob = require('glob');

// 특정 확장자를 가진 파일 검색 (*.txt)
glob('**/*.txt', (err, files) => {
  if (err) throw err;
  console.log('찾은 파일:', files);
});
```

<br/><br/>

### **`glob` 모듈의 주요 와일드카드**

| **패턴** | **설명** |
| --- | --- |
| `*` | 모든 파일 이름과 일치 (한 경로 레벨) |
| `**` | 모든 하위 디렉터리 포함 (재귀적으로) |
| `?` | 단일 문자와 일치 |
| `{a,b}` | `a` 또는 `b`와 일치 |
| `[abc]` | `a`, `b`, 또는 `c` 문자와 일치 |

<br/><br/>

### **`glob` 모듈의 주요 옵션**

| **옵션** | **설명** |
| --- | --- |
| **`ignore`** | 검색 결과에서 제외할 경로를 지정 <br/> 단일 패턴 또는 패턴 배열 사용 가능 |
| **`cwd`** | 검색을 시작할 현재 작업 디렉터리를 지정 <br/> 기본값 = `process.cwd()` |
| **`dot`** | `true`로 설정하면 숨김 파일(`.`으로 시작하는 파일)도 검색 결과에 포함 |
| **`nodir`** | `true`로 설정하면 디렉터리를 검색 결과에서 제외 |
| **`absolute`** | `true`로 설정하면 절대 경로로 검색 결과 반환 |
| **`follow`** | `true`로 설정하면 심볼릭 링크를 따라감 |
| **`matchBase`** | `true`로 설정하면 파일 이름만 기준으로 매칭(경로 무시) |
| **`nocase`** | `true`로 설정하면 대소문자를 구분하지 않음 |
| **`stat`** | `true`로 설정하면 파일의 통계 정보(`fs.stat()`)를 포함하여 반환 |
| **`sync`** | 동기식으로 동작하도록 설정 |

<br/>

**`ignore`** <br/>
: 특정 파일이나 디렉터리를 **검색 결과에서 제외**할 때 사용
```jsx
const glob = require('glob');

// 특정 파일 제외
glob('**/*.txt', { ignore: '**/test.txt' }, (err, files) => {
  if (err) throw err;
  console.log('검색된 파일:', files); // test.txt는 제외됨
});

// 특정 디렉터리 제외
glob('**/*', { ignore: '**/logs/**' }, (err, files) => {
  if (err) throw err;
  console.log('logs 디렉터리 제외 후 파일:', files);
});

// 여러 패턴 제외
glob('**/*.js', { ignore: ['**/node_modules/**', '**/*.test.js'] }, (err, files) => {
  if (err) throw err;
  console.log('검색된 JS 파일:', files); // node_modules와 테스트 파일 제외
});
```

<br/><br/>

**`cwd`** <br/>
: 검색 기준 디렉터리 지정
```jsx
const glob = require('glob');

// logs 디렉터리 내부의 .txt 파일 검색
glob('*.txt', { cwd: './logs' }, (err, files) => {
  if (err) throw err;
  console.log('logs 디렉터리 내 txt 파일:', files);
});
```

<br/><br/>

**`dot`** <br/>
: 숨김 파일 포함
```jsx
glob('**/*', { dot: true }, (err, files) => {
  if (err) throw err;
  console.log('숨김 파일 포함 결과:', files); // .env, .git 등 포함
});
```

<br/><br/>

**`nodir`** <br/> : 디렉터리 제외
```jsx
glob('**/*', { nodir: true }, (err, files) => {
  if (err) throw err;
  console.log('디렉터리 제외 결과:', files); // 파일만 출력
});
```

<br/><br/>

**`absolute`** <br/> : 절대 경로 반환
```jsx
glob('**/*.js', { absolute: true }, (err, files) => {
  if (err) throw err;
  console.log('절대 경로로 반환된 파일들:', files);
});
```

<br/><br/>

**`follow`** <br/> : 심볼릭 링크 따라가기
- 심볼릭 링크(symbolic link)
: 다른 파일이나 디렉터리의 경로를 가리키는 특수한 파일
    - 심볼릭 링크는 실제 파일이나 디렉터리의 참조 역할을 하며, 이를 통해 여러 위치에서 동일한 파일에 접근할 수 있다.
    - 예를 들어, 한 디렉터리 내에서 다른 디렉터리나 파일을 가리키는 링크를 만들 수 있다.
```jsx
glob('**/*', { follow: true }, (err, files) => {
  if (err) throw err;
  console.log('심볼릭 링크 포함 검색 결과:', files);
});
```

<br/><br/>

**`matchBase`** <br/>
: 경로 무시하고 파일 이름만 매칭
```jsx
glob('*.txt', { matchBase: true }, (err, files) => {
  if (err) throw err;
  console.log('경로 무시하고 파일 이름만 매칭:', files);
});
``` 

<br/><br/>

**`nocase`** <br/>
: 대소문자 무시

```jsx
glob('**/*.TXT', { nocase: true }, (err, files) => {
  if (err) throw err;
  console.log('대소문자 무시한 결과:', files);
});
```

<br/><br/>

**`stat`** <br/>
: 파일 통계 정보 포함
```jsx
glob('**/*.js', { stat: true }, (err, files) => {
  if (err) throw err;
  console.log('파일 경로와 통계 정보:', files);
});
```

<br/><br/>

**`sync`** <br/>
: 동기식 사용
```jsx
const files = glob.sync('**/*.txt');
console.log('동기식으로 검색된 파일들:', files);
```

<br/><br/>

**옵션 조합 사용**
```jsx
glob('**/*.txt', {
  cwd: './data',
  ignore: ['**/node_modules/**', '**/*.test.txt'],
  dot: true,
  nodir: true
}, (err, files) => {
  if (err) throw err;
  console.log('검색된 파일들:', files);
});
```

<br/><br/><br/>

### **✨ `fs` 모듈 vs `glob` 모듈**

| **기능** | **`fs` 모듈** | **`glob` 모듈** |
| --- | --- | --- |
| **주요 목적** | 파일 시스템 조작 (읽기, 쓰기, 삭제, 정보 확인 등) | 파일/디렉터리 검색 및 매칭 |
| **사용 방식** | 파일 또는 디렉터리 경로를 정확히 지정해야 함 | 와일드카드(`*`, `?`, `{}` 등) 패턴으로 검색 가능 |
| **비동기 지원** | 동기/비동기 방식 모두 지원 (`fs`와 `fs.promises`) | 비동기 방식 기본 제공 |
| **종속성** | Node.js 내장 모듈 | 별도 설치 필요 (`npm install glob`) |
| **주요 사용 사례** | 파일을 읽거나 쓰는 작업 | 특정 형식의 파일을 대량으로 검색 |

<br/><br/>
<br/>

---

# **`fs-extra` 모듈**
: **`fs` 모듈을 확장한 서드파티 라이브러리**
- 파일 시스템 작업을 더 간편하고 안전하게 처리할 수 있도록 추가적인 기능을 제공한다.
- **`fs` 모듈**에서 제공하는 기능들을 **더욱 직관적이고 강력하게** 만들 수 있는 여러 메서드가 추가된다.

<br/><br/>

### **✨ `fs`모듈 vs `fs-extra` 모듈**
| **기능** | **`fs`** | **`fs-extra`** |
| --- | --- | --- |
| **파일/디렉터리 읽기 및 쓰기** | 지원 | 지원 |
| **파일 복사** | 없음 | 지원 (`fs.copy`) |
| **디렉터리 생성** | `fs.mkdir` (이미 존재하면 에러 발생) | `fs.ensureDir` (존재 여부 관계없이 안전하게 생성) |
| **디렉터리 삭제** | `fs.rmdir` (빈 디렉터리만 삭제 가능) | `fs.remove` (빈 디렉터리 및 비어 있지 않은 디렉터리 모두 삭제 가능) |
| **파일 이동** | 없음 | 지원 (`fs.move`) |
| **파일 복사 시 덮어쓰기** | `fs.copyFile` (기본적으로 덮어씀) | 지원 (`fs.copy`는 옵션으로 덮어쓰기 설정 가능) |

<br/><br/>

## **`fs-extra` 모듈** 메서드
**`fs.copy(src, dest)`** <br/>
: 파일이나 디렉터리를 복사한다.
- **매개변수**
    - `src`: 복사할 원본 파일 또는 디렉터리 경로
    - `dest`: 복사한 파일 또는 디렉터리를 저장할 대상 경로
- **특징**: 대상 경로에 이미 존재하는 파일이 있으면 덮어쓴다.
```jsx
const fs = require('fs-extra');

fs.copy('source.txt', 'destination.txt')
  .then(() => console.log('파일 복사 완료'))
  .catch(err => console.error(err));
```

<br/><br/>

**`fs.ensureDir(path)`** <br/>
: 지정된 경로에 디렉터리가 존재하는지 확인하고, 없으면 디렉터리를 생성한다.
- **매개변수**
    - `path`: 생성할 디렉터리 경로
- **특징**: 디렉터리가 이미 존재하면 아무 작업도 하지 않는다.
```jsx
fs.ensureDir('./newDir')
  .then(() => console.log('디렉터리 생성 완료'))
  .catch(err => console.error(err));
```

<br/><br/>

**`fs.remove(path)`** <br/>
: 지정된 파일이나 디렉터리를 삭제한다.
- **매개변수**
    - `path`: 삭제할 파일 또는 디렉터리 경로
- **특징**: 비어 있지 않은 디렉터리도 삭제할 수 있다.
```jsx
fs.remove('./oldDir')
  .then(() => console.log('디렉터리 삭제 완료'))
  .catch(err => console.error(err));
```

<br/><br/>

**`fs.move(src, dest)`** <br/>
: 파일이나 디렉터리를 이동한다.
- **매개변수**
    - `src`: 이동할 원본 파일 또는 디렉터리 경로
    - `dest`: 이동할 대상 경로
- **특징**: 원본 파일/디렉터리를 삭제하고, 대상 경로로 이동한다.
```jsx
fs.move('oldDir', 'newDir')
  .then(() => console.log('디렉터리 이동 완료'))
  .catch(err => console.error(err));
```

<br/><br/>

**`fs.outputFile(path, data)`** <br/>
: 지정된 경로에 파일을 생성하고 데이터를 작성한다. (파일이 이미 있으면 덮어쓴다.)
- **매개변수**
    - `path`: 파일 경로
    - `data`: 파일에 쓸 데이터
- **특징**: 디렉터리가 없으면 자동으로 생성한다.
```jsx
fs.outputFile('newFile.txt', 'Hello, world!')
  .then(() => console.log('파일 작성 완료'))
  .catch(err => console.error(err));
```

<br/><br/>

**`fs.outputJson(path, object)`** <br/>
: 지정된 경로에 JSON 데이터를 작성한다.
- **매개변수**
    - `path`: JSON 파일 경로
    - `object`: JSON 데이터
- **특징**: 파일이 없으면 생성하며, 존재하면 덮어쓴다.
```jsx
const data = { name: 'John', age: 30 };

fs.outputJson('data.json', data)
  .then(() => console.log('JSON 파일 작성 완료'))
  .catch(err => console.error(err));
```

<br/><br/>

**`fs.readJson(path)`** <br/>
: 지정된 경로에서 JSON 파일을 읽어 객체로 반환한다.
- **매개변수**
    - `path`: 읽을 JSON 파일 경로
- **반환값**: JSON 데이터가 객체로 반환된다.
```jsx
fs.readJson('data.json')
  .then(data => console.log('읽은 데이터:', data))
  .catch(err => console.error(err));
```

<br/><br/>

**`fs.ensureFile(path)`** <br/>
: 지정된 경로에 파일이 있는지 확인하고, 없으면 새 파일을 생성한다.
- **매개변수**
    - `path`: 파일 경로
- **특징**: 파일이 없으면 새 파일을 생성하고, 있으면 아무 작업도 하지 않는다.
```jsx
fs.ensureFile('newFile.txt')
  .then(() => console.log('파일이 존재하거나 생성되었다.'))
  .catch(err => console.error(err));
```

<br/><br/>

**`fs.pathExists(path)`** <br/>
: 지정된 경로가 존재하는지 여부를 확인한다.
- **매개변수**
    - `path`: 확인할 경로
- **반환값**: 경로가 존재하면 `true`, 아니면 `false`를 반환한다.
```jsx
fs.pathExists('someDir')
  .then(exists => console.log(exists ? '경로 존재' : '경로 없음'))
  .catch(err => console.error(err))
```

<br/><br/>

**`fs.createFile(path)`** <br/>
: 지정된 경로에 파일이 없으면 새 파일을 만들고, 있으면 아무 작업도 하지 않는다.
- **매개변수**
    - `path`: 파일 경로
```jsx
fs.createFile('newFile.txt')
  .then(() => console.log('파일이 생성되었다.'))
  .catch(err => console.error(err));
```

<br/><br/>

**`fs.copySync(src, dest)`** <br/>
: 동기적으로 파일이나 디렉터리를 복사한다.
- **매개변수**
    - `src`: 복사할 원본 파일/디렉터리 경로
    - `dest`: 복사한 파일/디렉터리 경로
```jsx
fs.copySync('source.txt', 'destination.txt');

console.log('파일 복사 완료');
```

<br/><br/>

**`fs.removeSync(path)`** <br/>
: 동기적으로 파일이나 디렉터리를 삭제한다.
- **매개변수**
    - `path`: 삭제할 파일/디렉터리 경로
```jsx
fs.removeSync('oldDir');

console.log('디렉터리 삭제 완료');
```

<br/><br/>
<br/>
