* 基于hook的状态转换 hook + hook，体现hook状态组合和衍生能力
* useRequest给请求添加状态
  
```tsx [1-12|14-19|21-24]
import React, { useCallback } from 'react';
import { useRequest } from '@tms/site-hook';

const Demo = ({id}) => {
	const getUserList = useCallback((id) => 
		fetch(id).then(userList => userList), 
	[id])

	const { result, loading, error, run } = useRequest(getUserList);

	return loading ? null : result
}

const useUserList = (id) => {
	const getUserList = useCallback((id) => {
    	return fetch(id).then(userList => userList)
	}, [id])
	return useRequest(getUserList)
};// id => userList => loading + error + result

const Demo = ({id}) => {
	const { result, loading, error, run } = useUserList(id);
	return loading ? null : result
}

```