# lodash

> **lodash란?**  
>  JavaScript 유틸리티 라이브러리입니다. 배열, 개체, 문자열 및 숫자를 조작하기 위한 다양한 기능을 제공합니다. (출처: [Lodash](https://lodash.com/))


## lodash를 사용하는 이유
개발자로서 우리는 일반적으로 문제를 간편하게 해결하기 위해 외부 리소스를 사용하는 경향이 있습니다.  
문제 해결과 관련하여 유틸리티 라이브러리는 많은 문제를 해결할 수 있습니다.  
가장 인기 있는 유틸리티 라이브러리 중 하나는 lodash 입니다.


Lodash는 내장 객체를 확장하지 않고 함수형 프로그래밍 도우미를 제공합니다.  
`map`여기에는 전역 네임스페이스를 어지럽히지 않고 배열의 모든 요소에 함수를 적용하는 것과 같은 일반적인 기능 도우미가 포함되어 있습니다.


## API 요청 debounce
사용자가 무엇이든 입력할 수 있는 입력 필드가 있다고 가정해보자.  
사용자가 입력하고 변경 사항을 입력함에 따라 주어진 키워드에 대한 검색을 동시에 수행하려고 합니다.

그러나 사용자가 버튼을 누를 때마다 검색을 수행한다면 굉장히 많은 API 서버 요청으로 성능이  
저하될 여지가 있습니다.
아 문제는 `debounce`와 `throttle`을 활용해서 해결할 수 있습니다.  
위에 두가지에 대한 많은 의논이 있지만 이 부분은 다음에 설명하겠습니다.  
이번에는 lodash를 통한 `debounce`를 구현하겠습니다.

```DebounceInput.tsx
import { useState, useCallback } from "react";
import debounce from "lodash/debounce";

const DebounceInput = () => {
  const [inputValue, setInputValue] = useState("");

  const sendQuery = (query) => {
    // API콜을 진행
    console.log(query);
  };

  // 600ms의 딜레이를 걸어주는 useCallback함수
  const delayedSearch = useCallback(
    debounce((q) => sendQuery(q), 600),
    []
  );

  const handleChange = (event) => {
    // 입력 시 즉각적으로 내용 변경
    setInputValue(event.target.value);
    
    // 검색은 유저가 멈춘 순간에만 delayedSearch를 통해 API Call을 진행 
    delayedSearch(event.target.value);
  };

  return <input value={inputValue} onChange={handleChange} />;
};

export default DebounceInput;

```