---
boostnote:
  createdAt: '2019-10-21T01:38:46.650Z'
  updatedAt: '2019-10-21T08:02:26.004Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: ImagePagerAdapter
  tags:
    - Android
    - Java
  linesHighlighted: []
  isStarred: false
  key: b2d7b436-b71c-4eff-8d96-a5e6297d770b
  storage: d112f8ec1e85e056a09d
---

ImagePagerAdapter
---
network image view를 사용해서 이미지를 가져와 pager adapter 생성

```java
public class ImagePagerAdapter extends PagerAdapter {
    private Context mContext;
    private ArrayList<DataRecord> mEntries = null;

    ImagePagerAdapter(Context context, ArrayList<DataRecord> entries) {
        mContext = context;
        mEntries = entries;

        Log.v("공지사항:", ""+mEntries.size());
    }

    @Override
    public int getCount() {
        return mEntries.size();
    }

    @Override
    public boolean isViewFromObject(@NonNull View view, @NonNull Object object) {
        return view == object;
    }

    @NonNull
    @Override
    public Object instantiateItem(@NonNull ViewGroup container, final int position) {

        Log.v("공지사항 position:", ""+position);

        String imageDomain = mEntries.get(position).getImageDomain();
        String imagePath = mEntries.get(position).getImagePath();
        String imageName = mEntries.get(position).getImageName();
        String imageUrl = String.format("%s%s%s", imageDomain, imagePath, imageName);

        Log.v("공지사항 imageDomain:", ""+imageDomain);

//            String targetUrl = mEntries.get(position).getTargetUrl();

        NetworkImageView networkImageView = new NetworkImageView(mContext);
        networkImageView.setImageUrl(imageUrl, VolleySingleton.getInstance(mContext).getImageLoader());
        networkImageView.setScaleType(ImageView.ScaleType.FIT_XY);
        container.addView(networkImageView);

        networkImageView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
//                    alertMsg("서비스 준비중입니다./n - 홈페이지 -");
                String targetUrl = mEntries.get(position).getTargetUrl();
                Log.v("!!!", "targetUrl: " + targetUrl);

                Intent intent = new Intent(mContext, WebViewActivity.class);
                intent.putExtra("url", targetUrl);
                mContext.startActivity(intent);
            }
        });

        return networkImageView;
    }

    @Override
    public void destroyItem(@NonNull ViewGroup container, int position, @NonNull Object object) {
        container.removeView((ImageView) object);
    }
}

```