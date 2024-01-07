---
boostnote:
  createdAt: '2019-02-21T02:45:36.521Z'
  updatedAt: '2019-03-06T01:32:58.031Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: 하위 Fragment에서 상위 Fragment 가져오기
  tags:
    - Android
    - Java
  linesHighlighted: []
  isStarred: false
  key: ab6354ca-aff0-41a8-81e2-ae8fa0879d30
  storage: d112f8ec1e85e056a09d
---

하위 Fragment에서 상위 Fragment 가져오기
---
ParentFragment.java
```java
ChildFragment fragment = new ChildFragment();

FragmentTransaction transaction = getChildFragmentManager().beginTransaction();
transaction.replace(R.id.charge_container, fragment);
transaction.addToBackStack(null);
transaction.commit();
```

ChildFragment.java
```java
ParentFragment parentFragment = ((ParentFragment) ParentFragment.this.getParentFragment());
parentFragment.dismiss();
```