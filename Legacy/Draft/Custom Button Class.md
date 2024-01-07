---
boostnote:
  createdAt: '2019-01-16T02:47:21.976Z'
  updatedAt: '2019-01-17T01:49:22.768Z'
  type: MARKDOWN_NOTE
  folder: 69f287614baf36ef63db
  title: Custom Button Class
  tags: []
  isStarred: false
  key: 6b25906f-f458-4538-a142-59f982d75428
  storage: d112f8ec1e85e056a09d
---

Custom Button Class
---
```java
package co.kr.baristapaulbassett.paulbassett.ui.widget;

import android.content.Context;
import android.graphics.Typeface;
import android.graphics.drawable.GradientDrawable;
import android.util.AttributeSet;

import co.kr.baristapaulbassett.paulbassett.R;

/*
기본: 테두리 - 갈색, 바탕 - 투명, 텍스트 - 갈색
선택/사용 가능: 테두리 - 갈색, 바탕 - 갈색, 텍스트 - 흰색
사용 불가: 테두리 - 회색, 바탕 - 회색, 텍스트 - 흰색
ex. 카드 일반 충전 - 신용카드, 휴대폰, 카드 충전
 */
public class ButtonTwo extends android.support.v7.widget.AppCompatButton {

    GradientDrawable drawable;

    boolean mAvailableDisabled;

    public ButtonTwo(Context context) {
        super(context);

        drawable = new GradientDrawable();
        drawDefault();
    }

    public ButtonTwo(Context context, AttributeSet attrs) {
        super(context, attrs);

        drawable = new GradientDrawable();
        drawDefault();
    }

    public ButtonTwo(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);

        drawable = new GradientDrawable();
        drawDefault();
    }

    private void drawDefault() { // 기본
        setHeight(calcIntToDp(48));

        drawable.setShape(GradientDrawable.RECTANGLE);
        drawable.setStroke(1, getResources().getColor(R.color.brownOne));
        drawable.setCornerRadius(calcIntToDp(3));
        drawable.setColor(getResources().getColor(android.R.color.transparent));

        setTypeface(Typeface.create(getTypeface(), Typeface.NORMAL));
        setTextColor(getResources().getColor(R.color.brownOne));
        setTextSize(16);

        setBackground(drawable);
    }

    private void drawSelected() { // 선택/사용 가능
        setTypeface(getTypeface(), Typeface.BOLD);
        setTextColor(getResources().getColor(android.R.color.white));

        drawable.setStroke(0, getResources().getColor(R.color.brownOne));
        drawable.setColor(getResources().getColor(R.color.brownOne));

        setBackground(drawable);
    }

    private void drawDisabled() { // 사용 불가
        setTypeface(getTypeface(), Typeface.BOLD);
        setTextColor(getResources().getColor(android.R.color.white));

        drawable.setStroke(0, getResources().getColor(R.color.greyFive));
        drawable.setColor(getResources().getColor(R.color.greyFive));

        setBackground(drawable);
    }

    @Override
    public void setSelected(boolean selected) {
        super.setSelected(selected);

        if (selected) {
            drawSelected();
        } else {
            drawDefault();
        }
    }

    @Override
    public void setEnabled(boolean enabled) {
        super.setEnabled(enabled);

        if (enabled) {
            drawSelected();
        } else {
            drawDisabled();
        }
    }

    private int calcIntToDp(int dps) {
        final float scale = getContext().getResources().getDisplayMetrics().density;
        int pixels = (int) (dps * scale + 0.5f);

        return pixels;
    }
}
```