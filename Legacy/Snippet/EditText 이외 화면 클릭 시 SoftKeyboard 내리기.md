---
boostnote:
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: EditText 이외 화면 클릭 시 SoftKeyboard 내리기
  tags:
    - Android
  isStarred: false
  createdAt: '2018-01-22T02:20:48.592Z'
  updatedAt: '2019-03-06T01:33:49.893Z'
  linesHighlighted: []
  key: f6ebbc6fba57dc35dbf5
  storage: d112f8ec1e85e056a09d
---

EditText 이외 화면 클릭 시 SoftKeyboard 내리기
---
```java
private void hideKeyboard(View view) {
    if (!(view instanceof EditText)) {
        view.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                InputMethodManager imm = (InputMethodManager) getActivity().getSystemService(Context.INPUT_METHOD_SERVICE);
                imm.hideSoftInputFromWindow(view.getWindowToken(), 0);
            }
        });
    }

    if (view instanceof ViewGroup) {
        for (int i = 0; i < ((ViewGroup) view).getChildCount(); i++) {
            View innerView = ((ViewGroup) view).getChildAt(i);
            hideKeyboard(innerView);
        }
    }
}
```
위의 메소드를 `onCreateView`나 `onCreate`에서 부르기
```java
hideKeyboard(view.findViewById(R.id.linear_layout));
```
R.id.linear_layout은 최상위 뷰