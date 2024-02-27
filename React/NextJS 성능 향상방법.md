# NextJS 성능 향상

Next.js 렌더링 속도를 높이는 다양한 방법이 있습니다. 아래는 가장 일반적인 방법들입니다.

1. Static optimization: Next.js는 정적 페이지를 지원하므로, 가능한 페이지를 정적으로 생성하여 렌더링 속도를 향상시킬 수 있습니다.

2. Code splitting: 페이지마다 필요한 것만 불러오도록 코드를 분할하여 페이지 로딩 속도를 개선할 수 있습니다.

3. Lazy loading: 페이지에서 필요한 시점에만 리소스를 불러오도록 구현하여 렌더링 속도를 향상시킬 수 있습니다.

4. Image optimization: 이미지의 크기를 최적화하고, 적절한 포맷으로 압축하여 렌더링 속도를 향상시킬 수 있습니다.

5. Caching: 가능한 경우, 브라우저 캐시를 사용하여 리소스를 재사용하여 렌더링 속도를 향상시킬 수 있습니다.



## Static optimization
Next.js의 Static Optimization은 Next.js에서 제공하는 기능으로, 정적 페이지를 생성하여 렌더링 속도를 향상시킵니다.

Next.js에서는 정적 페이지를 생성하는 getStaticProps 메소드를 제공합니다. 이 메소드를 사용하면, 페이지가 처음 렌더링될 때 데이터를 가져와서 정적 페이지를 생성할 수 있습니다. 이후에는 정적 페이지가 제공되어, 브라우저에서 렌더링하여 페이지 로딩 속도를 향상시킬 수 있습니다.

예를 들어, 정적 페이지를 생성하는 경우 다음과 같은 코드를 작성할 수 있습니다.
```js
import { getStaticProps } from 'next';

function Page({ data }) {
  return <div>{data}</div>;
}

export const getStaticProps = async () => {
  const data = await fetchData();
  return {
    props: {
      data,
    },
  };
};

export default Page;

```

Static Optimization은 렌더링 속도뿐만 아니라, SEO 개선, 고정 URL 등에도 큰 도움이 됩니다. 그러므로 가능한 페이지에서 Static Optimization을 적용하는 것이 좋습니다/


## code splitting

Next.js의 Code Splitting은 Next.js에서 제공하는 기능으로, 애플리케이션의 JavaScript 코드를 여러 개의 작은 파일로 분할하여 로딩하는 것을 말합니다.

이는 브라우저에서 애플리케이션을 로딩할 때, 한 번에 로딩해야 할 큰 파일을 작은 파일로 분할하여 로딩하는 것으로, 페이지 로딩 속도를 향상시킬 수 있습니다.

Code Splitting은 Next.js에서 자동으로 처리됩니다. 예를 들어, 페이지 내에서 사용되는 컴포넌트는 필요할 때만 로딩되며, 페이지가 처음 렌더링될 때만 필요한 코드만 로딩하게 됩니다.

Code Splitting을 사용하면, 페이지 로딩 속도를 향상시키면서도 페이지 내에서 필요한 기능만 로딩하여, 사용자의 경험을 향상시킬 수 있습니다.

## Lazy loading

Next.js의 Lazy Loading은 애플리케이션의 성능을 향상시키는 기술 중 하나로, 페이지 내에서 필요한 컴포넌트만 로딩하는 것을 말합니다.

이는 브라우저에서 애플리케이션을 로딩할 때, 필요한 컴포넌트만을 로딩하고 나머지는 로딩하지 않는 것으로, 페이지 로딩 속도를 향상시킬 수 있습니다.

Lazy Loading을 사용하면, 페이지 내에서 필요한 컴포넌트만 로딩하게 되므로, 페이지 로딩 속도를 향상시키면서도 사용자의 경험을 향상시킬 수 있습니다.

Next.js에서 Lazy Loading을 사용하려면, dynamic 함수를 사용하여 필요한 컴포넌트를 동적으로 로딩하면 됩니다. 또한, 컴포넌트를 로딩할 때 React.lazy를 사용할 수도 있습니다.

## Image optimization

Next.js의 Image Optimization은 웹 페이지의 속도와 성능을 향상시키는 기술 중 하나로, 이미지의 용량을 최적화하여 브라우저에서 빠르게 로딩할 수 있도록 하는 것을 말합니다.

Image Optimization은 이미지의 크기, 품질, 포맷 등을 최적화하여, 브라우저에서 최적의 로딩 속도를 제공할 수 있도록 합니다. 이는 웹 페이지의 속도와 사용자 경험을 향상시킬 수 있습니다.

Next.js에서 Image Optimization을 사용하려면, 이미지를 압축하고, 적절한 포맷으로 변환하여 사용하면 됩니다. 또한, 이미지를 동적으로 로딩하여, 웹 페이지의 로딩 속도를 더욱 향상시킬 수 있습니다. 또한, next-optimized-images 라이브러리를 사용하여 Image Optimization을 구현할 수도 있습니다.

## caching

Next.js에서 caching은 웹 페이지의 리소스를 브라우저에 저장하여, 다음 로딩에 같은 리소스를 빠르게 불러오는 기술입니다. 이는 웹 사이트의 로딩 속도와 사용자 경험을 향상시키는 기술 중 하나입니다.

Next.js에서는 적절한 HTTP 헤더 설정을 통해 caching을 쉽게 설정할 수 있습니다. 예를 들어, 정적 파일이나 API 데이터와 같은 리소스는 장기간 캐싱을 설정하여, 사용자가 다시 접속할 때마다 빠르게 불러올 수 있도록 할 수 있습니다.

하지만, caching은 올바르게 설정하지 않을 경우, 오래된 데이터를 표시하거나, 웹 페이지의 최신 데이터와 맞지 않는 데이터를 표시하는 등의 문제가 발생할 수 있습니다. 따라서, caching 설정은 적절한 테스트와 디버깅이 필요합니다.