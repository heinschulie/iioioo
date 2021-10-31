
<script lang="ts">
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();

    import { BehaviorSubject, EMPTY, of, Observable } from 'rxjs';
    import { switchMap, expand, tap, delay, toArray } from 'rxjs/operators';

    export let backgroundColor = "var(--offWhite)";
    export let targetLetter = "";
    export let currentIdx = 0;
    export let alphabet = "abcdefghijklmnopqurstuvwxyz123456789"; 
    export let colormap = {}; 
    export let currentColorIndex = 1; 

    const alphabetArray = alphabet.split("");

    const get = (idx): Observable<{ idx: number, letter: string }> => {
        currentIdx = idx; 
        return of({ idx, letter: alphabetArray[idx] });
    } 

    let target = new BehaviorSubject('i'); 
    $: if(targetLetter) {
        target.next(targetLetter); 
    }
    
    const currentSymbol = target.pipe(
        switchMap((targetLetter: string) => {
                return get( currentIdx ).pipe(
                expand(({ idx, letter }) => {
                    // console.log("IN EXPAND: ", idx, letter); 
                    if(idx === alphabetArray.length - 1) {
                        idx = -1;
                    }
                    return letter !== targetLetter ? get(idx + 1).pipe( delay(50) ) : EMPTY
                })
            )
        })
    )

    function sayHello() {
        console.log("firing")
		dispatch('message', {
			text: 'Hello!'
		});
	}
    
    currentSymbol.subscribe(()=>{}, ()=>{}, sayHello)

</script>

{ $currentSymbol.letter }






