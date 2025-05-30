```
function useLatest<T>(value: T) {
  const ref = useRef(value);
  useEffect(() => {
    ref.current = value;
  }, [value]);
  return ref;
}


// 使用

const [count, setCount] = useState(0);
const latestCount = useLatest(count);

useEffect(() => {
    const interval = setInterval(() => {
      console.log('Count:', latestCount.current); // ✅ 永远是最新的 count
    }, 1000);

    return () => clearInterval(interval);
}, []);

// ......
```