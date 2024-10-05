# 01장 지식 탐구

PCB라는 도메인에 관한 지식 내용 

프로로타입이 구체적일 경우 도메인 전문가는 모델이 의미하는 바와 동작하는 소프트웨어와 모델 간의 관계를 좀더 명확하게 이해할 수 있었다.

새 기능을 설명할 때면 PCB 엔지니어에게 객체의 상호작용 방식에 관한 시나리오를 설명하게 했다.

**효과적인 모델링의 요소**

1. 모델과 구현의 연계
    - 초기 프로토타입을 토대로 본질적인 연결 고리를 만든 다음, 이어지는 모든 반복 주기 내내 그 연결 고리를 유지했다.
2. 모델을 기반으로 하는 언어 정제.
    - 누구라도 모델에서 바로 용어를 끄집어내어 모델의 구조와 일관되게 문장을 구성할 수 있게 됐고 별도의 해석 없이도 문장을 명확히 이해할 수 있었다.
3. 풍부한 지식이 담긴 모델 개발
4. 모델의 정제
5. 브레인스토밍과 실험

**지식 탐구**

지식 탐구는 혼자서 하는 활동이 아니다. 개발자와 도메인 전문가로 구성된 팀은 대체로 개발자가 이끄는 가운데 협업한다.

과거의 폭포수 개발 방식에서는 업무 전문가가 분석가에게 설명하고 다시 분석가는 업무 전문가가 설명한 내용을 이해하고 추상화해서 소프트웨어를 코드로 작성하는 프로그래머에게 넘긴다.

모델을 만들어 내는 데 따르는 모든 책임이 분석가에게 있으며, 이러한 모델은 오로지 업무 전문가가 알려주는 사항에만 근거한다.

프로그래머가 도메인에 관심이 없다면 그 이면에 숨겨진 원리는 알지 못한 채 애플리케이션에서 수행해야 할 사항만 습득한다.

애초부터 추상화를 시작해서 더욱 많은 일을 해낼 수 있는 모델로 발전시킬 것이다.

도메인 전문가의 지속적인 관여로 모델은 심층적인 업무 지식을 반영하고, 추상화된 개념은 참된 업무 원칙에 해당한다.

- 모델은 프로젝트 내내 계속해서 흘러가는 정보들을 조직화하는 도구로 자리잡는다.
- 모델은 요구사항 분석에 초점을 맞춘다.
- 모델은 프로그래밍과 설계와 밀접한 관계를 맺는다.
- 모델은 선순환을 통해 도메인에 대한 팀 구성원의 통찰력을 심화시켜 팀 구성원이 더욱 분명하게 모델을 파악하게 하고, 이는 한층 더 높은 수준으로 정제된 모델로 이어진다.

**지속적인 학습**

- 소프트웨어를 작성하기 시작할 때 우리는 충분히 알지 못한 상태에서 시작한다.
- 생산성이 매우 뛰어난 팀은 지속적인 학습을 바탕으로 의식적으로 지식을 함양한다.
- 개발자에게는 이것이 일반적인 도메인 모델링 기술과 기술적 지식이 모두 향상된다는 것을 의미한다.

**풍부한 지식이 담긴 설계**

도메인에 관련된 엔티티만큼 업무 활동과 규칙도 도메인에 중요한데, 어떠한 도메인에도 다양한 범주의 개념이 존재한다.

개발자는 모델의 변경에 맞춰 구현을 리팩터링해서 모델의 변경된 사항을 표현하고, 애플리케이션에서는 그러한 지식을 활용한다.

- 규칙을 명확하게 하고, 구체화하며, 조정하거나 고려해야 할 범위 밖으로 배제하는 것은 소프트웨어 전문가와의 긴밀한 협업 하에서 진행되는 지식 탐구를 통해 이뤄진다.

**선박 화물 운송 예약 애플리케이션**

- 예약 애플리케이션의 책임이 각 Cargo(화물)를 하나의 Voyage(운항)와 연관관계를 맺고, 그것을 기록/관리하는 것이라고 해보자.

```kotlin
fun makeBooking(cargo : Cargo, voyage : Voyage) : Int {
	val confirmation : Int = orderConfirmationSequence.next()
	voyage.addCargo(cargo, confirmation)
	return confirmation
}
```

선박이 운항 중에 나를 수 있는 화물의 최대치보다 예약을 더 받아들이는 것이 해운 산업의 관행이다. 이를 초과예약이라고 한다. 간혹 110% 예약과 같이 용적을 나타낼 때 퍼센트를 쓰기도 한다.

```kotlin
fun makeBooking(cargo : Cargo, voyage : Voyage) : Int {
	val maxBooking = voyage.capacity() * 1.1
	if((voyage.bookedCargoSize() + cargo.size) > maxBooking){
		return -1
	}
	
	val confirmation = orderConfirmationSequence.next()
	voyage.addCargo(cargo, confirmation)
	
	return confirmation
}
```

1. 코드가 작성된 대로라면 개발자의 도움이 있더라도 업무 전문가가 이 코드를 읽고 규칙을 검증하지는 못할 것이다.
2. 해당 업무에 종사하지 않고 기술적인 측면만 담당하는 사람은 코드와 요구사항을 결부시키기가 어려울 것이다.

**정책**

정책이란 잘 알려진 strategy 디자인 패턴의 또 다른 이름이다.

우리가 담고자 하는 개념은 정책이라는 의미와 잘 맞아 떨어지며, 이러한 정책도 똑같이 도메인 주도 설계의 중요한 동기에 해당한다.

```kotlin
fun makeBooking(cargo : Cargo, voyage : Voyage) : Int {
	if(!overBookingPolicy.isAllowed(cargo, voyage)){
		return -1
	}
	
	val confirmation = orderConfirmationSequence.next()
	voyage.addCargo(cargo, confirmation)
	
	return confirmation
}

fun isAllowed(cargo : Cargo, voyage : Voyage) : Boolean{
	return (cargo.size + voyage.bookedCargoSize()) <= (voyage.capacity * 1.1)
}
```

이렇게 하면 초과예약이 별개의 정책이라는 사실을 모든 이가 분명히 알게 될 것이며, 이 규칙의 구현 또한 명시적으로 드러나고 다른 구현과 분리된다.

**심층 모델**

유용한 모델은 겉으로 드러나 있는 경우가 거의 없다.

해운 업무를 바라보는 시각이 장소 간 컨테이너의 이동에서 엔티티와 엔티티 사이의 화물에 대한 책임 이동으로 바뀐 것이다. 아울러 이러한 책임 이동을 취급하는 것과 관련된 기능이 더는 선적 작업에 부자연스럽게 붙어 있는 것이 아니라 작업과 책임 간의 중요한 관계를 토대로 만들어진 모델의 중심에 자리잡았다.

지식 탐구는 탐험과도 같아서 어디서 끝나게 될지 알지 못한다.