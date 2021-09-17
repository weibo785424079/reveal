* 组合示例，输入金额显示税款
* 状态防抖 => 异步请求 => 状态
  
```tsx
import React, { useCallback } from 'react';
import { Input } from 'antd'
import { useRequest, useDebounce } from '@tms/site-hook';

const Demo = ({id}) => {
	
    const [value, setValue] = useState(0); // 输入金额

    const debouncedVal = useDebounce(value); // 金额防抖

    const getResult = useCallback((id) => fetch(num).then(num => num), [debouncedVal])

    const {result, loading} = useRequest(getResult) // 衍生状态

	return (
        <Spin spining={loading}>
            <div>
                输入金额：<Input value={value} onChange={e => setValue(e.target.value)} />
                应收税款：{result}
            </div>
        </Spin>
    )
}

```