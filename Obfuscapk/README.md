# Obfuscapk: 안드로이드 앱 자동 난독화 도구

**Obfuscapk**는 안드로이드 앱(.apk)을 소스 코드 없이, 블랙박스 방식으로 자동으로 난독화하는 도구입니다. 모듈식 아키텍처를 채택하여 새로운 기술을 쉽게 추가하고 확장할 수 있습니다.

---

## **❱ 원본 레포지토리**

[Mobile-IoT-Security-Lab/Obfuscapk: An automatic obfuscation tool for Android apps that works in a black-box fashion, supports advanced obfuscation features and has a modular architecture easily extensible with new techniques](https://github.com/Mobile-IoT-Security-Lab/Obfuscapk?tab=readme-ov-file)

---

## **❱ 설치**

Obfuscapk를 사용하는 가장 효율적인 방법은 **Docker**를 이용하는 것입니다. Docker는 실행에 필요한 모든 환경을 이미지 안에 패키징하여, 복잡한 설치 과정 없이 즉시 프로그램을 실행할 수 있게 해줍니다. 

```jsx
$ git clone https://github.com/ClaudiuGeorgiu/Obfuscapk.git
```

### Docker image

---

1. **사전 준비**

먼저 로컬 컴퓨터에 Docker가 설치되어 있는지 확인하세요.

```jsx
docker --version
```

**2.    Docker 이미지 다운로드 및 준비**

**Official Docker Hub image**

Docker Hub에서 official Obfuscapk Docker image 접근

![image.png](attachment:b9c2c0b0-b260-41de-b639-c1cc01a80298:image.png)

Docker Hub에 있는 공식 Obfuscapk 이미지를 다운로드하고 사용하기 쉬운 이름으로 태그를 지정합니다.(터미널에서 실행)

```bash
 # 1. Docker Hub에서 공식 이미지를 다운로드합니다.
docker pull claudiugeorgiu/obfuscapk

# 2. 'obfuscapk'라는 짧은 이름으로 태그를 지정합니다.
docker tag claudiugeorgiu/obfuscapk obfuscapk
```

1. **설치 확인**

Docker Hub에서 offical image 다운 받았다면,  Obfuscapk/src directory에 Docker image build

```bash
# Make sure to run the command in Obfuscapk/src/ directory.
# It will take some time to download and install all the dependencies.
 docker build -t obfuscapk .
```

Docker image가 준비되었다면, 아래 명령어를 확인하여 잘 설치되었는지 확인합니다(도움말 메시지가 정상적으로 출력된다면 설치가 완료된 것입니다)

```bash
docker run --rm -it obfuscapk --help
```

usage: python3 -m obfuscapk.cli [-h] -o OBFUSCATOR [-w DIR] [-d OUT_APK_OR_AAB]
...

---

## **❱ Docker 역할**

### 💡 Git Clone vs. Docker Image

- **`git clone`으로 받은 소스 코드**: 
프로그램의 상세한 **"설계도"** 와 같습니다. 코드를 직접 분석하고 수정할 수 있지만, 실행을 위해서는 개발 환경(OS, 언어, 라이브러리 등)을 직접 구축해야 합니다.
- **`docker pull`로 받은 이미지**: 
실행에 필요한 모든 환경과 부품이 포함된 **"자동화 조립 공장"** 과 같습니다. `docker run` 명령어 하나로 즉시 프로그램을 실행할 수 있어 매우 편리합니다

---

## **❱사용법(Usage)**

---

### ## Obfuscapk 작동의 핵심 원리: 난독화 기법 + 마무리 작업

Obfuscapk의 작동 원리는 간단합니다. **"어떤 APK 파일을"**, **"어떤 난독화 기술(Obfuscator) 순서로"** 처리할지 지정하면 됩니다.
어떤 APK 파일을 어떤 순서로 난독화할지 `-o` 옵션으로 알려줘야 합니다.

- **`obfuscator`**: 실제 난독화 기술입니다. (예: `RandomManifest`, `Rename` 등)
- **마무리 작업 3총사**: 어떤 난독화를 하든, 최종적으로 설치 가능한 APK 파일을 만들려면 **반드시** 아래 3가지 작업이 마지막에 포함되어야 합니다.
    1. **`o Rebuild`**: 수정된 내용을 바탕으로 APK 파일을 다시 조립합니다.
    2. **`o NewAlignment`**: 조립된 APK 파일을 최적화(align)합니다.
    3. **`o NewSignature`**: 앱을 설치할 수 있도록 서명(sign)합니다.

즉, 명령어의 구조는 항상 `(실제 난독화 기술들) + (마무리 3총사)` 형태가 됩니다.

### ## 요약 및 다음 단계 💡

- 항상 **별도의 작업 폴더**에서 시작하세요.(난독화를 진행할 때 마다 새로운 폴더를 만들어서 실행)
- 명령어는 **`[도커 실행부] [옵션] [난독화 기술] [마무리 3총사] [원본 파일]`** 구조를 따릅니다.
- 이제 `o RandomManifest` 부분을 다른 난독화 기술로 바꿔가며 어떤 변화가 생기는지 테스트해볼 수 있습니다. 여러 개의 난독화 기술을 동시에 적용할 수도 있습니다.

---

### ## 실전 예제: APK 파일 난독화 따라하기

**1. 작업 폴더 준비하기** 📂

- C 드라이브에 `apk_test` 라는 새 폴더를 만드세요. (경로: `C:\apk_test`, 새로운 난독화 진행 시 새로운 폴더를 만드세요)
- 난독화하고 싶은 APK 파일을 이 폴더에 복사해 넣으세요. 파일 이름은 `original.apk`라고 가정하겠습니다.
- 이제 `C:\apk_test` 폴더에는 `original.apk` 파일 하나만 있습니다.

**2. 터미널에서 작업 폴더로 이동하기**

- PowerShell이나 cmd를 열고 `apk_test` 폴더로 이동합니다.

       PowerShell

```powershell
cd C:\apk_test
```

**3. 명령어 조립 및 실행하기** 

- 이제 문서에 나온 예제(`RandomManifest`)를 사용해 명령어를 만들어 보겠습니다.
- `d` 옵션을 추가해서 결과 파일 이름을 `obfuscated.apk`로 깔끔하게 지정해 줄게요.

아래 명령어를 복사해서 터미널에 붙여넣고 실행하세요.

PowerShell

```jsx
docker run --rm -it -v  "C:\apk_test:/workdir" obfuscapk -d obfuscated.apk -o RandomManifest -o Rebuild -o NewAlignment -o NewSignature original.apk
```

오류 발생 시 --use-aapt2 추가해서 실행

```powershell
 docker run --rm -it -v "C:\apk_test:/workdir" obfuscapk --use-aapt2 -d renamed.apk -o Rename -o Rebuild -o NewAlignment -o NewSignature original.apk
```

**명령어 분석:**

- **`docker run ... -v "C:\apk_test:/workdir" obfuscapk`**: 도커를 실행하고, 현재 폴더(`C:\apk_test`)를 컨테이너의 작업 폴더와 연결합니다.
- **`d obfuscated.apk`**: 결과물 파일 이름을 `obfuscated.apk`로 지정합니다.
- **`o RandomManifest`**: 'AndroidManifest.xml' 파일의 내용을 무작위로 섞는 난독화를 적용합니다.
- **`o Rebuild -o NewAlignment -o NewSignature`**: 수정된 앱을 다시 조립하고, 최적화하고, 서명합니다.
- **`original.apk`**: 난독화를 적용할 원본 파일입니다.

**4. 결과 확인하기** ✅

- 명령어를 실행하면 터미널에 많은 로그가 올라오면서 작업이 진행됩니다.
- 작업이 성공적으로 완료되면 `C:\apk_test` 폴더 안에 **`obfuscated.apk`** 라는 새로운 파일이 생성된 것을 볼 수 있습니다. 이 파일이 바로 난독화가 완료된 최종 결과물입니다.

---

### 요약

- 항상 **별도의 작업 폴더**에서 시작하세요.(난독화를 진행할 때 마다 새로운 폴더를 만들어서 실행)
- 명령어는 **`[도커 실행부] [옵션] [난독화 기술] [마무리 3총사] [원본 파일]`** 구조를 따릅니다.
- 이제 `o RandomManifest` 부분을 다른 난독화 기술로 바꿔가며 어떤 변화가 생기는지 테스트해볼 수 있습니다. 여러 개의 난독화 기술을 동시에 적용할 수도 있습니다.

---

### 사용 가능한 난독화 기술 (`-o` 옵션)

> **'ArithmeticBranch', 
'CallIndirection', 
'DebugRemoval',
 'Goto',
 'MethodOverload',
 'Nop',
 'Reflection',
 'Reorder', 
 'AssetEncryption',
 'ConstStringEncryption',
 'LibEncryption',
 'ResStringEncryption', 
 'VirusTotal',
 'ClassRename**
> 

---

### ## 명령어 정리

`docker run --rm -it -u $(id -u):$(id -g) -v "${PWD}":"/workdir" obfuscapk [params...]`

- `docker run`: 도커 이미지를 실행하라는 기본 명령어입니다.
- `-rm`: 명령 실행이 끝나면 컨테이너(임시 작업 공간)를 자동으로 삭제해서 깔끔하게 유지합니다.
- `it`: 실행 과정을 터미널에서 실시간으로 보고 상호작용할 수 있게 해줍니다.
- `u $(id -u):$(id -g)`: **(리눅스/macOS 전용)** 컨테이너 안에서 파일을 생성할 때 내 컴퓨터의 사용자 권한과 동일하게 만듭니다. **Windows에서는 이 부분이 오류를 일으키므로 보통 생략합니다.**
- `v "${PWD}":"/workdir"`: **가장 중요한 부분입니다!**
    - `v`는 볼륨(volume)을 의미하며, 내 컴퓨터의 폴더와 도커 컨테이너의 폴더를 연결하는 '포털'을 만드는 것과 같습니다.
    - `"${PWD}"`: **내 컴퓨터의 현재 폴더**를 의미합니다. (Windows에서는 `"%CD%"`를 사용합니다.)
    - `"/workdir"`: **도커 컨테이너 안의 작업 폴더**입니다.
    - 즉, **"현재 내가 있는 폴더를 컨테이너의 `/workdir` 폴더에 연결해줘!"** 라는 뜻입니다. 이렇게 해야 컨테이너 안의 Obfuscapk가 내 APK 파일을 읽을 수 있습니다.
- `obfuscapk`: 실행할 도커 이미지의 이름입니다.
- `[params...]`: Obfuscapk의 실제 기능(옵션)을 지정하는 부분입니다. `-help`로 봤던 내용들이 여기에 들어갑니다.

---

### ## 주요 옵션 사용법 알아보기

- `-help`를 통해 본 옵션들 중 가장 기본적이고 필수적인 것들은 다음과 같습니다.
- `<APK_OR_BUNDLE_FILE>`: **(필수)** 난독화할 원본 APK 파일의 이름입니다. 명령어의 가장 마지막에 위치합니다.
- `o <OBFUSCATOR>`: **(필수)** 적용할 난독화 기술의 이름을 지정합니다. 여러 기술이 있으며, 가장 간단한 예로는 `Rename` (클래스/메서드 이름 바꾸기), `AssetEncryption` (에셋 파일 암호화) 등이 있습니다.
- `d <OUT_APK_OR_AAB>`: **(선택, 추천)** 결과물이 저장될 파일의 이름을 지정합니다. 지정하지 않으면 `obfuscated.apk`와 같이 기본 이름으로 저장됩니다.
- `w <DIR>`: 작업 디렉토리를 지정하지만, 우리는 위에서 `v` 옵션으로 작업 폴더를 이미 연결했기 때문에 거의 사용할 일이 없습니다.

---
