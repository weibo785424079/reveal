* 获取dom滚动位置

```tsx [8]
import React, { useRef } from 'react';
import { useScroll } from '@tms/site-hook';

const list = Array(100).fill('x');

const Demo = () => {
  const ref = useRef<HTMLDivElement>(null);
  const {left, top} = useScroll(ref);
  return (
    <>
      <div>
        {left,top}
      </div>
      <div ref={ref} style={{ height: 200, width: 400, overflow: 'scroll' }}>
        {
            list.map((x, i) => <div key={i} style={{ height: 20, width: 600 }}>{x}</div>)
        }
      </div>
    </>
  );
};

```
* 一行代码获取元素滚动位置