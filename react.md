[Accueil](README.md)
# REACT 

### Framwork react :
- Next.js
- Time stack (ensemble de library dont React)

### Major update to know
- 16.0 : Les Fragments
- 16.3 : Context API
- 16.8 : Les Hooks
- 18.2 : Concurrent management
- 19.1 : Aujourd’hui

**Render** action of create the dom from the code.
Concept of **virtual dom**, React create a virtual dom witch serve as comparative when there are a change in the page to only render the difference.

## Component 
The component is composed of 3 thingd : **Props**, **State**, **Render**

A component have 3 state in the life cycle :
**Mounting** -> **Updating** -> **Unmounting**

The fragment `<></>` permit to have a list of element in my component without create a wrapper (ex: div). 

Classic component :
```tsx
interface MyButtonProps {
	color: string;
}

function MyButton(props: MyButtonProps) {
	const [enabled, setEnabled] = useState<boolean>(true);
	...
	return (
		<button>I'm a button</button>
	);
}
```


## Hook
- Function preceed by **use** (only a convention)
- Used at the star of the component only
- Can't be used on condition or a loop
- Only on the function (not class)

### `useState`

```javascript
const [enabled, setEnabled] = useState<boolean>(true);
```

Use only when it's really necessary !!
In the future React Compiler will erase the necessary of that.

### `useEffect`
```javascript
useEffect(function, dependencyArray): void;
```

Ex :
#### Mount 
```javascript
useEffect(() => {
	doWhenMount();
}, []);
```

#### Update
```javascript
useEffect(() => {
	doForSpecificUpdate();
}, [props.something]);
```

```javascript
useEffect(() => {
	doForEachUpdate();
});
```

#### Unmount
```javascript
useEffect(() => {
	return () => {
		doWhenUnmount();
	};
});
```

```javascript
const memoizedValue = **useMemo**(() => computeExpensiveValue(a, b), [a, b]);
```

```javascript
const memoizedCallback = **useCallback**(
	() => {
		doSomething(a, b);
	},
	[a, b],
);
```

Not re-render ! Permit to do native api function on the element (like: scrollIntoView, ...)

```javascript
const intervalRef = **useRef**(0);
```


## Other 

### Key
When I have a list of item i need to use a key prop to permit

### HOC
deprecated with hooks

### Context
L’api Context pour partager des
données entre composants dans
l’arbre et éviter le prop drilling


### Redux
- Redux est une librairie de gestion d’état
- Redux possède un Store
- Composant dispatch des actions
- Actions sont souvent des appels externes
- La réponse est envoyé aux Reducers
- Le Reducer est une fonction pure qui génère un nouveau store depuis le précédent et la réponse

### Tanstack-query
new hook 
```javascript
const { data, isError, isPending } = **useQuery**<Article[]>({
	queryKey: [QueryKey.ARTICLES],
	queryFn: () => findArticles(),
});
```
