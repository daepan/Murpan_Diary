Computed, Watch
----------------
둘다 반응형 프로퍼티로 이 둘의 사용에 대하여 많은 혼동을 얻기도 한다.  
그렇다면 이에 대한 차이점과 사용법에 대해 자세히 작성해 보자  

<h2>   Computed - 반응형 getter </h2>
Computed에 정의하는 함수는 반드시 값을 리턴하여야한다.

<h3>getter란?</h3>
computed에서 함수를 한 프로퍼티가 정의될때 내부적으로는 `Object.defineProperty`로 정의되며 이때 
함수는 getter로 설정된다. ????

```
    위 내용을 정리하자면 defineProperty라는    
    객체의 속성을 정교하게 추가하거나 수정할 수 있는 객체 메서드를 이용하여
     computed에 정의된 함수를 getter로 지정하여 계산결과를 캐싱할 수 있는 특성을
    지니게 된 것이다.
``` 

이때 정의된 함수는 아래와 같은 특징을 가지게 된다.
 <li>함수가 아닌 일반 객체로 사용할 수 있다</li>
 <li>호출될 때만 계산이 이루진다</li>
 <li>계산결과가 캐싱되는 특성을 지니게 된다.</li>
   
  마지막에 작성된 캐싱되는 특성은 getter의 특성 때문이다.  
  위 내용 때문에 값이 변하게 되어도 캐싱에 의해 변경된 값을 인지하지 못하는 단점을 지니고 있다.  
  
  <h3>반응형</h3>
  Vue에서는 반응형을 구현하기 위해 getter 함수 내에 속한 프로퍼티의 변경여부를 추적하는 특별한
  기능을 이용하게 된다.  
  즉 , 변수 하나를 감시하면서 있다가 변경된다면 이에 관련된 함수를 다시 계산한다.  
  `computed`는 사용하기 편하고, 자동으로 값을 변경하고 캐싱해주는 "반응형 `getter`"라고 한다.
  
  <h2>Watch  -  반응형 콜백</h2>
  변경을 감시(watch)한다는 특징을 지닌다는 것에서 computed와 watch를 혼동할 수 있다.
  확실한 차이점으로 `watch`는 Vue인스턴스의 특정 프로퍼티가 변경될 때 지정한 콜백함수가
  실행 되는 기능이다.  
  
 ```
<template>
  <div>
    <p>원본 메시지: "{{ message }}"</p>
    <p>역순으로 표시한 메시지: "{{ reversedMessage }}"</p>
  </div>
</template>

<script>
export default {
  name: 'test',
  data(){
    return {
      message: '안녕하세요',
      reversedMessage: ''
    }
  },
  watch: {
    message: function (newVal, oldVal) {
      this.reversedMessage = newVal.split('').reverse().join('')
    }
  }
}
</script>
```

  `watch`로 정의된 부분에서 `message` 프로퍼티가 익명함수로 할당되어 콜백함수의 역할을 하게 된다  
     <li>`message` 프로퍼티의 값이 변경되게 된다면 변경된 값을 콜백함수의 첫번째 인자</li> 
     <li>이전 값을 두번째 인자로 보내준다.</li>
     <br>    
        
  이렇게 `watch`는 아무런 프로퍼티도 생성하지 않고
  콜백함수로 이용하여 처리한다.
  
  <h3>언제 무엇을 사용해야하나요?</h3>
  <li>data에 존재하는 값들이 종속적인 속성이 있을 경우 computed를 이용하는 것이 좋다.</li>
   <li>watch는 특정 프로퍼티의 변경시점에 특정액션을 취하고자 할때 적합하다.</li>
   <li>하지만 일반적으로 computed로 구현가능한 것은 `watch`보다는 computed가 옳다</li>
     
  
  
  
    
   

