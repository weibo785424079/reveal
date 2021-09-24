* useVirtualList dom + 事件 + 数据

```tsx [1-30|32-39]
import React from 'react';
import { useVirtualList } from '@tms/site-hook';

const Item = ({ text, index } : {text: string, index: number}) => (
  <div
    style={{ height: 50, background: index % 2 === 0 ? '#ccc' : 'transparent' }}
  >
    {text}
  </div>
);

const data = Array.from({ length: 100 }, (_, index) => index);

export default () => {
  const {
    list,
    containerProps,
    wrapperProps,
  } = useVirtualList(data, { itemHeight: 50 });

  return (
    <div {...containerProps} style={{ height: '300px', overflow: 'auto' }}>
      <div {...wrapperProps}>
        {
              list.map((i) => <Item key={i.index} index={i.index} text={String(i.data)} />)
          }
      </div>
    </div>
  );
};

useEffect(() => {
  const listener = () => console.log('滚动')

  ref.current.addEventListener('scroll', listenner)

  return () => ref.current.removeEventListener('scroll', listenner)

}, [])


```
* 体现hook访问dom的便捷，自定义hook可以抛出ref或者外部传入ref，使用自由
* 基于ref 访问dom，绑定事件，逻辑更聚合