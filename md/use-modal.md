* 封装弹窗状态visible，useModal
* 组件也可以hook
  
```tsx [1-18|19-55]
import React from 'react';
import { useModal, useImmutable } from '@tms/site-hook';

const Demo = () => {
  const { show, hide, RenderModal, visible } = useModal<{ hobby: string }>({
    title: '标题',
    okText: '关上~',
    cancelText: '取消',
  });
  return (
    <div>
      <Button onClick={() => show({ hobby: '汽车' })}>打开弹窗</Button>
      <RenderModal>
        <div>我是弹窗内容!</div>
      </RenderModal>
    </div>
  );
};

function Demo({ hobby }: { hobby: string }) {
  return <p>爱好: {hobby}</p>;
}

const useXXX = () => {
  const { RenderModal, hide, ...rest } = useModal<{ hobby: string }>({
    title: '标题',
    okText: '关上~',
    cancelText: '取消',
    onOk() {
      console.log('您点击了确定');
      hide();
    },
    onCancel() {
      console.log('您点击了取消');
      hide();
    }
  });
  const Render = useImmutable(() => {
    return <RenderModal>{(props: { hobby: string }) => <Demo {...props} />}</RenderModal>;
  });
  return {
    ...rest,
    Render
  };
};

const Demo3 = () => {
  const { show, Render } = useXXX();
  return (
    <>
      <Button onClick={() => show({ hobby: '篮球' })}>打开弹窗</Button>
      <Render />
    </>
  );
};

```
* 声明使用，无需在主逻辑代码中声明而外state，或ref命令式打开弹窗