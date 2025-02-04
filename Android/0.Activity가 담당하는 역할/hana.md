# 1. Activity가 담당하는 역할

공홈 설명에 따르면 Activity는 다음과 같음

1. 사용자와의 상호작용을 하는 하나의 UI
   Activity마다 각각의 UI를 표시하고 이 UI를 통해서 사용자와 상호작용을 한다

2. 독립적인 컴포넌트

   - 각각의 Activity는 독립적으로 동작하고, 각 Activity마다 자신만의 life cycle을 가지고 있음

3. Entry Point

- app을 실행할 때 메인액티비티에서 시작됨
- 다른 app에서 특정 기능을 호출할 때 해당 기능의 entry point가 됨
  (ex: 글 작성할 때 사진 촬영 -> 카메라 activity 실행 -> 사진 촬영 끝 -> 다시 글 작성 activity로 돌아옴)

꼬리질문)

- Activity간에 데이터를 어떻게 주고 받나요?
  Activity간의 데이터 전달은 Intent를 사용함
  저의 경우 카메라 액티비에서 받아온 uri값을 setData(uri)를 통해 intent에 담고, 그 intent를 setResult로 반환해서 사용함 다른 방법으로는 putExtra("String"), getStringExtra()를 사용하는 방법도 있다고 함
