* 基于hook的状态转换 hook + hook，体现hook状态组合和衍生能力
* useRequest给请求添加状态，十分灵活
  
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

---

```tsx
// useRequest 源码
import { useCallback, useEffect, useReducer, useRef } from 'react';

interface Options {
  immediate: boolean;
  manul: boolean;
  delay: number;
  handleResult: (...args: any[]) => any;
}

export type PromiseType<T> = T extends Promise<infer P> ? P : T;

interface Result<R, T extends (...args: any[]) => any> {
  run: T;
  loading: boolean;
  error: Error | undefined;
  result: R;
}

const defaultHandeResult = <T>(d: T) => d;

function useRequest<R = any, T extends(...args: any[]) => any = (...args: any[]) => any>(fn: T, options: Partial<Options> = {}): Result<R, T> { // eslint-disable-line
  const optionRef = useRef(
    (() => {
      const { manul = false, immediate = true, delay = 1000, handleResult = defaultHandeResult } = options;
      return {
        manul,
        delay: delay ?? 1000,
        immediate: manul ? false : immediate,
        handleResult
      };
    })()
  );

  const runRef = useRef<T>(fn);

  runRef.current = fn;

  const isCurrent = useCallback((cb: T) => runRef.current === cb, []);

  const timer = useRef<number | null>(null);

  const initValue = { loading: false, result: undefined, error: undefined };

  const [value, dispatch] = useReducer((state: Result<R, T>, action) => ({ ...state, ...action }), initValue);

  const openLoading = useCallback(() => {
    const { delay } = optionRef.current;

    if (timer.current) {
      clearTimeout(timer.current);
    }
    if (delay) {
      timer.current = window.setTimeout(() => {
        dispatch({ loading: true });
      }, delay);
    } else {
      dispatch({ loading: true });
    }
  }, []);

  const closeLoading = useCallback((closeOptions = {}) => {
    if (timer.current) {
      clearTimeout(timer.current);
    }
    dispatch({ loading: false, ...closeOptions });
  }, []);

  const run = useCallback(
    async (...args) => {
      try {
        openLoading();

        const result = await fn(...args);

        if (isCurrent(fn)) {
          closeLoading({
            loading: false,
            error: undefined,
            result: optionRef.current.handleResult(result)
          });
        }
      } catch (error) {
        if (isCurrent(fn)) {
          closeLoading({ loading: false, error, result: undefined });
        }
        console.log(error);
      }
    },
    [fn]
  );

  useEffect(() => {
    const { manul, immediate } = optionRef.current;
    if (!manul) {
      if (immediate) {
        run();
      } else {
        optionRef.current.immediate = true;
      }
    }
    return () => {
      if (timer.current) {
        clearTimeout(timer.current);
      }
    };
  }, [run]);

  return { run, ...value };
}
// 本质是依赖一个被useCallback包裹的函数，
// 函数执行前后 可以做到延时loading，自动请求, 解决竞态, 返回结果和错误
export default useRequest;

```