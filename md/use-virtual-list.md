* useVirtualList dom + 事件 + 数据

```tsx
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
```
* 体现hook访问dom的便捷
* 绑定dom事件
* 根据dom状态封装数据的能力