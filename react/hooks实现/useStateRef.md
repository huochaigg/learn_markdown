```
function useStateRef<T>(initial: T): [T, (v: T) => void, React.MutableRefObject<T>] {
  const [state, setState] = useState(initial);
  const ref = useRef(state);

  const set = useCallback((v: T) => {
    ref.current = v;
    setState(v);
  }, []);

  return [state, ref, set];
}
```