# Next/Image

### Next/Image 컴포넌트의 기능

- Next/Image 컴포넌트에서 제공하는 대표적인 기능은 다음의 3가지이다.
  - lazy loading
    - 이미지 로드하는 시점을 필요할 때까지 지연시키는 기술. 예를 들면 스크린 밖에 있는 이미지들은 로딩을 지연시키고 스크린 안에 있는 이미지만을 로드해서, 불필요한 대역폭 사용을 줄이고 필요한 이미지만 빠르게 로드할 수 있다.
  - 이미지 사이즈 최적화
    - Next/Image는 디바이스 크기 별로 srcSet을 미리 지정해두고, 사용자의 디바이스에 맞는 이미지를 다운로드할 수 있게 지원한다. 또한 Next.js는 이미지를 webp와 같은 용량이 작은 포맷으로 이미지를 변환해서 제공한다.
    - 사용자의 디바이스에 맞는 이미지 사이즈를 만들고, 용량이 작은 webp 포맷으로 변환하는 작업은 이미지에 대한 최초 요청 시에 Next.js 서버에서 진행된다. 이후 요청에는 캐시가 만료될 때까지 캐시 된 이미지가 제공되기 때문에 첫 번째 요청보다 훨씬 빠르게 이미지를 서빙할 수 있다. 캐시가 만료된 후 요청이 들어오면 오래된 이미지를 우선 제공하고, 백그라운드에서 이미지 최적화를 다시 진행한다.
  - placeholder 제공
    - 어떤 웹 사이트에 방문했을 때 이미지가 로드되기 전까지 영역의 높이가 0이었다가 이미지가 로드된 후 이미지만큼 영역이 늘어서 레이아웃이 다시 그려지는 경우가 있다. 이를 CLS(Cumulative Layout Shift)라고 부른다. Next/Image는 레이아웃이 흔들리는 현상을 방지하기 위해 placeholder를 제공한다. 이미지가 로드되기 전에도 이미지 높이만큼 영역을 표시해서 이미지가 로드된 후에 레이아웃이 흔들리지 않도록 하는 것이다. placeholder는 빈 영역 또는 blur 이미지(로컬 이미지의 경우 build 타임에 생성, 리모트 이미지의 경우에는 base64로 인코딩된 data url 을 지정해 줘야 함)로 적용할 수도 있고, 커스텀 하게 설정할 수도 있다.

### Next/Image를 사용하면서 얻게 되는 장점

- 성능 향상: 디바이스마다 적절한 사이즈의 이미지를 서빙하고, webp와 같은 작은 용량의 포맷을 사용함
- 시각적인 안정성: 이미지 로드 전 placeholder를 제공하여 CLS(Cumulative Layout Shift) 방지
- 빠른 페이지 로딩: viewport에 들어왔을 때만 이미지를 로드하고, 작은 사이즈의 blur 이미지를 미리 로딩하여 사용자에게 더 빠른 페이지를 보여줄 수 있음

### 사용법

- 로컬 이미지의 경우 빌드 타임에 import 된 이미지 파일을 기준으로 자동으로 width, height 지정하고, base64로 인코딩된 blur 이미지가 생성되어 별도의 작업 없이 placeholder=“blur”를 사용할 수 있다.

```
import Image from 'next/image';
import profilePic from '../public/me.png';

function Me() {
  return (
    <Image
      src={profilePic}
      alt="Picture of me"
      placeholder="blur" // Optional blur-up while loading
    />
  );
}
```

- 리모트 이미지의 경우 Next.js 서버에서 이미지를 가지고 있는 리모트 서버에 직접 요청을 하기 때문에 모든 url에 대한 접근을 허용할 경우 악의를 가진 사용자에 의해 공격을 받을 가능성이 있다. 이를 방지하기 위해 이미지를 서빙하는 서버가 안전한 서버라는 것을 Next.js에 알려줘야 하는데, next.config.js 파일에 CDN의 host를 명시해야 한다.

```
// next.config.js
module.exports = {
  images: {
    domains: ['your-cdn-image-domain'],
  },
};
```

- 이미지 파일을 import 하는 대신에 src에 이미지 경로를 작성해야 한다. 로컬 이미지와 달리 리모트 이미지의 경우에는 빌드 타임에 이미지 파일에 접근하는 것이 불가능하기 때문에 width, height 정보를 기입해 줘야 하고, blur 이미지도 생성되지 않는다. 이미지가 로드되기 전 placeholder로 blur 이미지를 사용하고 싶은 경우에는 별도로 blurDataURL 속성에 base64로 인코딩된 이미지 데이터를 작성해 줘야 한다.

```
import Image from 'next/image';

export default function Me() {
  return <Image src="/me.png" alt="Picture of me" width={500} height={500} />;
}
```
