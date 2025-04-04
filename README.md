# 만든 이유

yes24 페이지를 하루에도 여러번 하도 들어가다 보니, 귀찮아서 만들어야지 하다가! AI 도움?!을 받아서 완성 했습니다!

yes24를 워낙 자주 쓰기에, 만들어 보았습니다.

교보문고 , 알라딘도 엮을까 싶지만, 두 곳의 접속 비율이 1년에 1-2번이라 yes24만 택했습니다.

사용은 아주 간편합니다!

키워드는 yes24 입력하고 스페이스 책 제목 입력하면 됩니다!

![](yes24.png)

사실, 일 하다가 심심해서 만들어 보았습니다. 기타 버그는 시간 되면 고치겠습니다.

물론, 기타 수정 사항도 나중에 고치겠습니다.





아! 중요한게, 알프레드 워크플로우는 한글이 깨지므로, nfc 방식의 처리가 필요합니다. NFD -> NFC 처리가 필요합니다.

https://jmjeong.com/unicode-in-alfred-workflow/

**alfred workflow의 한글 처리**

Mac에서 alfred workflow를 만들다보면 한글 argument 비교 처리가 안 되는 현상이 있습니다. Mac에서 unicode를 NFD(Normalization Form Decomposition)으로 처리를 해서 생기는 문제입니다. Windows나 python 등에서는 NFC로 처리가 됩니다.

jmjeong.com

여기 블로그의 도움을 많이 받았습니다.

NFD와 NFC는 유니코드 정규화(normalization) 형식의 두 가지 방법인데, 한글 처리에서 중요한 차이를 가지고 있습니다.

**NFD (Normalization Form Decomposition)**

- **특징**: 문자를 분해하여 기본 문자와 결합 문자로 분리합니다

- **한글에서**: 초성, 중성, 종성으로 완전히 분리됩니다

- **예시**: "한글" → "ᄒ", "ᅡ", "ᆫ", "ᄀ", "ᅳ", "ᆯ"

**NFC (Normalization Form Composition)**

- **특징**: 가능한 한 문자를 결합하여 단일 코드 포인트로 표현합니다

- **한글에서**: 초성, 중성, 종성이 하나의 완성형 글자로 결합됩니다

- **예시**: "ᄒ", "ᅡ", "ᆫ", "ᄀ", "ᅳ", "ᆯ" → "한글"

**왜 한글이 깨지는가?**

한글은 초성(19개), 중성(21개), 종성(27개)의 조합으로 이루어져 있는데, 유니코드에서는 두 가지 방식으로 이를 표현할 수 있습니다:

1. **완성형 한글**: 각 글자가 하나의 코드 포인트로 표현됩니다 (U+AC00 ~ U+D7A3)

2. **조합형 한글**: 자모 낱개가 각각의 코드 포인트로 표현됩니다 (U+1100 ~ U+11FF, U+3130 ~ U+318F)

예를 들어, "부자들의"는 NFC에서는 4개의 완성형 글자로 표현되지만, NFD에서는 10개의 자모로 분리됩니다. 만약 시스템이 이 차이를 제대로 처리하지 못한다면, 한글이 깨져 보이게 됩니다.

**Alfred Workflow에서의 문제**

Alfred Workflow에서는 JSON 데이터 처리 과정에서 맥 OS가 기본적으로 NFD를 사용하는 경향이 있어, API에서 받은 완성형 한글(NFC)이 자모 분리형(NFD)으로 변환될 수 있습니다. 이런 상태로 출력하면 사용자 인터페이스에서 한글이 깨져 보일 수 있습니다.

unicodedata.normalize('NFC', text) 함수를 사용하면 이런 분리된 자모를 다시 완성형 한글로 결합하여 올바르게 표시할 수 있습니다.

이런 정규화 과정은 한글뿐만 아니라 악센트가 있는 라틴 문자 등 다양한 문자 체계에서도 중요한 역할을 합니다.
