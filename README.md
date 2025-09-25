#  멀티플랫폼 난독화 도구 프로토타입 (Multi-platform Obfuscation Tool Prototype)

**숙명여자대학교 SW중심대학사업단 & 익스트러스(Extrus) 산학협력 프로젝트**

<br>

##  프로젝트 소개 (About The Project)

최근 모바일 앱을 대상으로 한 리버스 엔지니어링 및 해킹 위협이 증가함에 따라, 코드와 핵심 로직을 보호하는 앱 보안 기술이 필수적으로 요구되고 있습니다.

본 프로젝트는 \*\*Android(Java/Kotlin)\*\*와 **iOS(Swift/Objective-C)** 양대 모바일 플랫폼을 모두 지원하는 **멀티플랫폼 난독화 솔루션 프로토타입**을 개발하는 것을 목표로 합니다. 소스 코드를 분석하고 변환하여 클래스, 함수, 문자열 등 주요 식별자를 식별하기 어렵게 만들어 앱의 구조적 보안을 강화합니다.

<br>

##  주요 기능 (Features)

  * **멀티플랫폼 지원**: Android (Java/Kotlin) 및 iOS (Swift/Objective-C) 소스 코드 동시 지원
  * **정적 코드 난독화**: 빌드 타임에 소스 코드를 분석하고 변환
  * **다양한 난독화 타겟**:
      * 클래스 및 함수명 변경 (Renaming)
      * 상수 문자열 암호화 (String Encryption)
      * 제어 흐름 위조 (Control Flow Obfuscation)
  * **CLI 기반 도구**: 사용자가 직접 난독화 옵션을 설정하고 실행할 수 있는 명령줄 인터페이스 제공

<br>

##  주요 기술 스택 (Tech Stack)

  * **Platforms**: Android, iOS
  * **Core Technologies**: LLVM, ProGuard
  * **File Analysis**: Mach-O (for iOS)

<br>

##  추진 내용 (Development Plan)

1.  **리서치 및 분석**:

      * Android 및 iOS 플랫폼의 코드 구조 및 빌드 프로세스 분석
      * 주요 리버스 엔지니어링 기법 및 취약점 조사

2.  **설계 및 구현**:

      * LLVM, ProGuard, Mach-O 등 관련 기술을 기반으로 플랫폼별 난독화 로직 설계
      * 각 난독화 타겟(이름, 문자열, 제어 흐름)에 대한 변환 알고리즘 구현

3.  **도구 개발**:

      * CLI(Command-Line Interface) 기반의 독립 실행 프로그램 개발
      * 사용자가 난독화 수준과 방식을 설정할 수 있는 옵션 기능 구현

4.  **분석 및 측정**:

      * 난독화 적용 전/후의 앱 성능(실행 속도, 용량 등) 영향 분석
      * 보안성 측정 지표를 통해 난독화 효과 정량적 평가

5.  **테스트 및 검증**:

      * 실제 상용 앱을 대상으로 개발된 프로토타입 적용 테스트
      * 안정성 및 호환성 실증 검증

<br>

##  최종 목표 및 결과물 (Goals & Deliverables)

✅ **Android/iOS 멀티플랫폼 난독화 도구 프로토타입**

> 독립적으로 실행 가능한 프로그램 형태로, CLI를 통한 설정 기능을 포함합니다.

✅ **기술 보고서 (Technical Report)**

> 아래 내용을 포함한 종합 기술 문서 1부:
>
>   * 개발된 도구의 상세 설계 및 아키텍처
>       * 적용된 난독화 기술별 보안성 분석 결과
>       * 난독화 전후 성능 비교 데이터

<br>

##  참여 기관 (Partners)

  * **[숙명여자대학교 SW중심대학사업단](https://sw.sookmyung.ac.kr/)**
  * **익스트러스 (Extrus)**
