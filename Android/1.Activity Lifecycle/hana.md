# 1. Activity Life cycle에 대해서 설명하세요

1. onCreate()

- activity가 처음 생성될 때 호출됨(activity 전체 라이프 사이클 중 단 한 번만 발생)
- 프래그먼트의 초기 생성을 수행하기 위해 호출되며 초기 레이아웃 생성(setContentView), data binding 작업을 주로 함(초기화에 적합)
- 매개변수로 savedInstanceState가 있음 -> 처음 생성된 activity는 savedInstanceState가 null임
  그러나 액티비티가 재생성되었을 떄(ex: 화면 회전 등)는 상태정보가 savedInstanceState에 있음. 이 savedInstanceState를 통해 다시 시작된 액티비티를 이전 상태로 복원할 수 있음
- 반드시 구현해야하는 callback method임

2. onRestart()

- Activity가 stop상태(onStop()이 호출된 상태)에서 다시 실행상태로 복귀할 때 호출되는 메소드
  (ex: app 실행(onResume() 인 상태) -> 홈 버튼 클릭 -> onPause() -> onStop() -> 다시 app으로 돌아감 -> onRestart() -> onStart() -> onResume())
- 쉽게 생각하면 사용자가 앱을 완전히 종료하지 않고 잠시 다른 곳으로 벗어났다가 다시 돌아올 때 실행(앱이 백그라운드에 있다가 포그라운드로 돌아오는 경우)
- 강제종료는 안 됨

3. onStart()

- onCreate(), onRestart()가 시작되면 실행되는 콜백메소드
- Activity가 화면에 보이기 시작할 때 호출(단, 사용자와의 상호작용은 못하는 상태임)
- onStart() 후에 Activity가 포그라운드에 표시되면 onResume(), 안 보이면 onStop()이 호출됨
- ex) 앱 처음 실행할 때 : onCreate() -> onStart() -> onResume()
- ex) 백그라운드에서 다시 돌아올 때 : onRestart() -> onStart() -> onResume()
- 네트워크나 무거운 리소스(ex: 카메라, 애니메이션)등을 로딩하는 작업

4. onResume()

- 사용자와 상호작용할 수 있는 상태
- 이 단계에서 activity stack의 맨 위에 위치하게 됨
- 자원들(ex: audio, video, animation. camera...)을 실행함
- 사용자가 다른 activity로 이동하거나 기기의 화면이 꺼지거나 혹은 기기를 회전하는 등의 포커스 변화가 발생하기 전까지 앱은 onResume 상태를 유지함.
  만약 activity가 화면에 보이지 않는 상태가 되면 onPause()가 무조건 실행됨

5. onPause()

- 포커스를 다른 activity가 가져갈 때 호출됨
- activity가 일시중지되어 onResume()에서 했던 리소스들(오디오, 애니메이션 등)을 여기서 중단해야함(액티비티가 백그라운드에 있는 동안 필요하지 않은 자원들을 해제함)
- onPause()는 안드로이드에 의해서 항상 실행이 보장됨

6. onStop()

- activity가 사용자에게 전혀 보이지 않게 되면 호출됨
- 안드로이드 시스템에서 자원이 급하게 필요하게 되면 onStop()가 실행 중이어도 강제종료될 수 있음
- onStop()은 호출되지 않을 수도 있기 때문에 이 부분을 고려해서 코드를 작성해야함
- activity가 다시 실행된다면 onRestart()가 실행되고, 종료된다면 onDestroy()가 실행됨

7. onDestroy()

- activity가 종료될 때(ex: finish() 호출 등) 자동으로 호출됨
- onCreate(Bundle)에서 했던 작업을 정리하고 해제함
- 정상적인 종료인 경우에는 isFinishing = true, 아니라면 false

꼬리질문)

1. 휴대폰을 세로에서 가로 모드로 바꾼다면 안드로이드 라이프 사이클은 어떻게 작동하나요?
   -> 나라면 이렇게 답변할거같음(근데 틀릴수도... 피드백 주세요!)
   onPause() -> onStop() -> onDestroy() -> onCreate()에서 저장된 위치 복원 -> onStart() -> onResume() 순으로 다시 호출된다

2. 휴대폰에서 영상을 재생 중일 때 가로 모드로 바꾸어도 어떻게 영상이 계속 재생이 되는걸까요?
   onPause()에서 현재 재생 위치(시간)을 저장함 -> onSaveInstanceState(Bundle)에서 재생된 위치(시간)을 번들에 저장함 -> onStop() -> onDestroy() -> onCreate()에서 번들값이 null이 아니라면(저장된 상태가 있다면) 재생위치(시간) 복원 -> onStart() -> onResume() 순으로 동작함

3. onStart와 onResume의 차이점은?
   onStart는 액티비티가 화면에 보이기 시작할 때 실행되거나 혹은 백그라운드에서 다시 포그라운드로 앱이 돌아왔을 때 onRestart() 다음으로 실행됨
   onResume()은 액티비티가 사용자와 상호작용이 가능한 상태일 때 호출됨

4. onPause와 onStop()의 차이점은?
   onPause()는 액티비티가 부분적으로 보이지만 포커스를 잃었을 때(ex: 다이얼로그가 액티비티 위에 나타날 때 등) 호출되고 onStop()은 현재 액티비티가 사용자에게 완전히 보이지 않을 때(다른 액티비티가 호출되서 보이고 있다든지 등) onPause -> onStop이 실행됨
   또한 onPause는 반드시 호출이 보장되지만 onStop은 호출되지 않을 수도 있음(안드로이드 시스템에서 리소스가 급하게 필요하거나 회수해야하는 경우)

5. onCreate에서 저장된 상태를 복원할 때 주의할 점은?
   savedInstanceState가 null인지 체크. 왜냐하면 액티비티가 재생성될 때만 값이 있기 때문

6. activity가 백그라운드에서 종료된 후 다시 시작될 때 어떻게 처리할 것인지?
   메모리가 부족하거나 하는 등의 경우로 백그라운드에 있는 액티비티가 종료된다면 savedInstanceState에 저장하고 onCreate에서 Bundle에 있는 데이터를 불러와서 다시 액티비티가 시작할 때 데이터를 복원해서 사용할 것임
