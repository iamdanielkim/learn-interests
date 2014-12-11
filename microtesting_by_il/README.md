
# Three Dimensions Of Microtesting

* https://www.dropbox.com/s/8z1vj201tjp4r5x/Three_Dimensions_Of_Microtesting.png?dl=0


# Do you mean we should achieve 100% test coverage?

* Not 100% Coverage
    * Coverage는 아무것도 말해주지 않는다.
     (정말 의미 있는지, 리스키한지..)
* Not 1:1 Cost, Test mapping
    * class의 기능성 또는 행위를 테스트하는데 집중하는게 필요
* Not Catch every possible error
    * 들어가는 공력이 큰만큼의 항상 그만큼의 결과를 얻는건 아니다. 무엇일 테스트하는게 중요한지 잘 판단하는 것이 필요

* 권장하는 Minimum coverage?
    * 치팅없이 할수있는 만큼

# Interesting: Worth investing time and energy in microtesting.

* 여기저기 많이 사용되는 기본 코드
* 코드의 가장 중요한 행위
* 잠재적으로 리스크가 큰 연산
* 막연하게 불안한 것들
* 복잡한 연산
* 코딩 외적으로 깨지기 쉬운 부분

**Example**
```
Collection libraries, utility methods
A List is a collection that keeps items in order
Calculate a billing amount
billing amount is rounded down when adding the sales tax
Averaging a sequence of numbers
Loading invoice data from the database
```


(23P) 코드를 주고 무엇을 테스트해야할까?














