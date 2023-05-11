# React Hook Form

* [Controlled vs Uncontrolled](#controlled-vs-uncontrolled)

## Controlled vs Uncontrolled
1. defaultValue 설정
    - React Hook Form에서 input을 사용하는 경우, 제어 방식과 비제어 방식에 따라 defaultValue를 설정하는 방법이 다름. 사실 공식 문서에서는 reset API를 사용하는 것을 권장하지만, 비제어 방식인 경우 굳이 리렌더링을 trigger하는 reset API를 사용하지 않고도 defaultValue를 설정할 수 있음
    1. 제어 방식
        ```jsx
        import { useState, useEffect } from 'react';

        import { useForm, Controller } from 'react-hook-form';

        const App = () => {
            // ============= Server Data =============
            const [data, setData] = useState(undefined);
            useEffect(() => {
                const timer = setTimeout(function () {
                    setData('success');
                }, 50);

                return () => {
                    clearTimeout(timer);
                };
            }, []);

            // ============= React Hook Form =============
            const { control, handleSubmit, reset } = useForm({
                defaultValues: {
                    test: '', // defaultValues 속성에 test가 정의되어 있어야 제어 방식으로 인식하여 오류없이 reset이 가능
                },
            });
            useEffect(() => {
                if (data) {
                    reset({
                        test: data,
                    });
                }
            }, [data, reset]);
            const onSubmit = (data) => {
                console.log(data);
            };

            console.log('render'); // 총 3번 render

            return (
                <form onSubmit={handleSubmit(onSubmit)}>
                    <Controller
                        name='test'
                        control={control}
                        render={({ field: { value, onChange } }) => (
                            <input
                                value={value}
                                onChange={(e) => {
                                    onChange(e.target.value);
                                }}
                            />
                        )}
                    />
                    <button type='submit'>제출</button>
                </form>
            );
        };

        export default App;
        ```
    1. 비제어 방식
        ```jsx
        import { useState, useEffect } from 'react';

        import { useForm } from 'react-hook-form';

        const App = () => {
            // ============= Server Data =============
            const [data, setData] = useState(undefined);
            useEffect(() => {
                const timer = setTimeout(function () {
                    setData('success');
                }, 50);

                return () => {
                    clearTimeout(timer);
                };
            }, []);

            // ============= React Hook Form =============
            const { register, handleSubmit } = useForm();
            const onSubmit = (data) => {
                console.log(data);
            };

            console.log('render'); // 총 2번 render

            return (
                <form onSubmit={handleSubmit(onSubmit)}>
                    <input
                        {...register('test')}
                        ref={data && register('test').ref} // ref 속성값이 nullish일 때(최초 mount 시), 내부적으로 defaultValue 속성에 값을 할당하지 않음
                        defaultValue={data}
                    />
                <button type='submit'>제출</button>
                </form>
            );
        };

        export default App;
        ```

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#react-hook-form)