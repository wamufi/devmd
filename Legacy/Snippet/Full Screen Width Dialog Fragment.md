---
boostnote:
  createdAt: '2018-12-05T02:01:27.755Z'
  updatedAt: '2019-03-06T01:33:33.473Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Full Screen Width Dialog Fragment
  tags:
    - Android
    - Java
    - UI
  isStarred: false
  linesHighlighted: []
  key: 5746b3c8-42ce-47e1-b214-102e31a99980
  storage: d112f8ec1e85e056a09d
---

Full Screen Width Dialog Fragment
---
```java
public class FullScreenWidthDialog extends DialogFragment {

    @NonNull  
    @Override  
    public Dialog onCreateDialog(@Nullable Bundle savedInstanceState) {  
        Dialog dialog = super.onCreateDialog(savedInstanceState);  
        dialog.getWindow().requestFeature(Window.FEATURE_NO_TITLE);  

        return dialog;  
    }  

    @Nullable  
    @Override  
    public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {  
        return inflater.inflate(R.layout.fragment_dialog, container, false);  
    }
    
    @Override  
    public void onViewCreated(@NonNull View view, @Nullable Bundle savedInstanceState) {
        // Customizing View
    }

    @Override  
    public void onStart() {  
        super.onStart();  

        Dialog dialog = getDialog();  
        if (dialog != null) {  
            dialog.getWindow().setLayout(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.MATCH_PARENT);  
            dialog.getWindow().setBackgroundDrawable(new ColorDrawable(Color.WHITE));  
        }
    }
}
``````