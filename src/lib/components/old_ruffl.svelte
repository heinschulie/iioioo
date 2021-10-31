<!-- 
<script lang="ts">

    import Letter from '$lib/components/Letter.svelte'; 

    import { BehaviorSubject, from, of } from "rxjs"; 
    import { map, switchMap, concatMap, delay } from "rxjs/operators"; 
    export let backgroundColor = "var(--offWhite)";

    export let firstString = "";
    export let secondString = "";
    export let alphabet = "abcdefghijklmnopqurstuvwxyz123456789"; 
    export let colormap = {}; 
    export let currentColorIndex = 1; 

    const firstArray = firstString.split("");
    const secondArray = secondString.split("");
    const alphabetArray = alphabet.split("");

    let currentTargetArray = new BehaviorSubject('first');

    function mapOutPath (idx: number, target: string, arr: string[]) {
        
        let path = [];
        while(arr[ idx ] !== target) {
            idx = (idx === arr.length - 1) ? 0 : (idx + 1); 
            path.push(alphabetArray[ idx ]);
        }
        return path;
    }

    let xmap = firstArray.reduce((acc, firstLetter, idx) => {
        
        const secondLetter = secondArray[ idx ]; 
        const firstIndex = alphabetArray.indexOf(firstLetter);
        const secondIndex = alphabetArray.indexOf(secondLetter);

        let pathToSecond = mapOutPath(firstIndex, secondLetter, alphabetArray); 
        let pathToFirst = mapOutPath(secondIndex, firstLetter, alphabetArray); 
        
        const sequence = currentTargetArray.pipe(
            concatMap(val => val === 'first' ? of( from(pathToFirst) ) : of( from (pathToSecond) ) ),
            // switchMap(val => of(val)),
            // delay(800)
        )
        
        return { ...acc, [ idx ]: { sequence, pathToSecond, pathToFirst } };

    }, {});

    console.log("XMAP: ", xmap)

    xmap[0].sequence.subscribe(val => {
        console.log("VALUE IN XMAP: ", val); 
    })

    // const letterMap = currentValue.pipe(
    //     switchMap(val => {
    //         return from( val.map((i, idx) => singleruffl(idx, secondArray) ) ); 
    //     })
    // )

    // initialiseMap(firstArray); 

    // export let keyLetters = [ "i", "i", "o", "i", "o", "o"];
    // export let alphabet = "abcdefghijklmnopqurstuvwxyz"; 

    // function shuffleArray(array) {
    //     let curId = array.length;
    //     // There remain elements to shuffle
    //     while (0 !== curId) {
    //         // Pick a remaining element
    //         let randId = Math.floor(Math.random() * curId);
    //         curId -= 1;
    //         // Swap it with the current element.
    //         let tmp = array[curId];
    //         array[curId] = array[randId];
    //         array[randId] = tmp;
    //     }
    //     return array;
    // }

    // let items = shuffleArray( alphabet.split('') );
    // console.log("shuffling madness:", items);
    // let map = items.reduce((acc, curr, idx) => {
    //     // return { ...acc, [ idx ]: curr }
    //     return keyLetters.some(key => key === curr) ? { ...acc, [ idx ]: curr } : acc;
    // }, {})

    // let map = keyLetters.reduce((acc, curr, idx) => ({ ...acc, [ idx ]: curr }), {})
    
    // console.log("MAP: ", map);

    // function sleep(ms) {
    //     return new Promise( fulfil => setTimeout(fulfil, ms) )
    // }

    // const singleruffl = async (idx: number, targetArray: string[]) => {
        
    //     if(idx === 0) {
    //         console.log("STARTING SINGLERUFFL in ", secondString); 
    //     };

    //     let runningIdx = idx; 
    //     let target = targetArray[idx]; 

    //     while(map[ idx ] !== target) {
    //         runningIdx = (runningIdx === alphabetArray.length - 1) ? 0 : (runningIdx + 1); 
    //         // map[ idx ] = alphabetArray[ runningIdx ];
    //         await sleep(80);
    //         return alphabetArray[ runningIdx ];
    //     }

    // }

    // const ruffl = (sourceArray: string[], targetArray: string[]): Promise<void>[] => {
    //     return sourceArray.map((i, idx) => singleruffl(idx, targetArray) ); 
    // };

    const windup = async () => {
        // initialiseMap(firstArray); 
        // await ruffl(firstArray, secondArray);
        // check(secondArray); 
        currentTargetArray.next('second');
    }

    const winddown = async () => {
        // initialiseMap(secondArray); 
        // await ruffl(secondArray, firstArray);
        // check(firstArray); 
        currentTargetArray.next('first');
    }

    // const check = (targetArray: string[]) => {
    //     targetArray.forEach((letter, idx) => {
            
    //         if(map[idx] !== letter) {
    //             singleruffl(idx, targetArray); 
    //         }
    //         else {
    //             console.log("IN CHECK: ", map[idx], letter);
    //         }
    //     })
    // }

    const activate = (idx: number) => {
        return colormap[currentColorIndex] && colormap[currentColorIndex].includes(idx); 
    }

</script>

<section style="grid-auto-columns: {100/firstArray.length}%" on:mouseenter={windup} on:mouseleave={winddown}>

    {#each firstArray as item, idx}
    {#if xmap[idx]}
    <span style="background-color: {backgroundColor};" class="block" class:active={activate(idx)}>
        <h1>{ xmap[idx] }</h1>
    </span> 
    <Letter letter={ xmap[idx] } />
    {/if}
    {/each}
    
    <p>{currentTargetArray}</p>   
</section>


<style lang="scss">

    @import '../design/breakpoints.scss';

    section {
        // margin-bottom: 2px;
        display: grid;
        grid-auto-flow: column;
        height: 33%;
        position: relative; 
        // gap: 2px;
        // margin-bottom: 2px;
        // align-content: center;
        // justify-content: center;
        // justify-items: center;
    }
    section p {
        position: absolute; 
    }
    // .block {
    //     padding: 10px;
    //     width: 100%;
    //     background-color: red;
    // }
    h1 {
        font-size: 10em;
        font-weight: bold;
        width: 100%;
        text-align: center;
        margin: 0; 
    }

    .active {
        h1 {
            color: red; 
        }
    }
    

</style> -->




<!-- 
<script lang="ts">

    import { BehaviorSubject, from } from "rxjs"; 
    import { map, switchMap } from "rxjs/operators"; 
    export let backgroundColor = "var(--offWhite)";

    export let firstString = "";
    export let secondString = "";
    export let alphabet = "abcdefghijklmnopqurstuvwxyz123456789"; 
    export let colormap = {}; 
    export let currentColorIndex = 1; 

    const firstArray = firstString.split("");
    const secondArray = secondString.split("");
    const alphabetArray = alphabet.split("");

    // let map = new BehaviorSubject({});
    // let map = {};
    let currentValue = new BehaviorSubject(firstArray);
    const initialiseMap = (array) => {
        return array.reduce((acc, curr, idx) => {
            return { ...acc, [ idx ]: curr }
        }, {});
    }

    const letterMap = currentValue.pipe(
        switchMap(val => {
            return from( val.map((i, idx) => singleruffl(idx, secondArray) ) ); 
        })
    )

    initialiseMap(firstArray); 

    // export let keyLetters = [ "i", "i", "o", "i", "o", "o"];
    // export let alphabet = "abcdefghijklmnopqurstuvwxyz"; 

    // function shuffleArray(array) {
    //     let curId = array.length;
    //     // There remain elements to shuffle
    //     while (0 !== curId) {
    //         // Pick a remaining element
    //         let randId = Math.floor(Math.random() * curId);
    //         curId -= 1;
    //         // Swap it with the current element.
    //         let tmp = array[curId];
    //         array[curId] = array[randId];
    //         array[randId] = tmp;
    //     }
    //     return array;
    // }

    // let items = shuffleArray( alphabet.split('') );
    // console.log("shuffling madness:", items);
    // let map = items.reduce((acc, curr, idx) => {
    //     // return { ...acc, [ idx ]: curr }
    //     return keyLetters.some(key => key === curr) ? { ...acc, [ idx ]: curr } : acc;
    // }, {})

    // let map = keyLetters.reduce((acc, curr, idx) => ({ ...acc, [ idx ]: curr }), {})
    
    // console.log("MAP: ", map);

    function sleep(ms) {
        return new Promise( fulfil => setTimeout(fulfil, ms) )
    }

    const singleruffl = async (idx: number, targetArray: string[]) => {
        
        if(idx === 0) {
            console.log("STARTING SINGLERUFFL in ", secondString); 
        };

        let runningIdx = idx; 
        let target = targetArray[idx]; 

        while(map[ idx ] !== target) {
            runningIdx = (runningIdx === alphabetArray.length - 1) ? 0 : (runningIdx + 1); 
            map[ idx ] = alphabetArray[ runningIdx ];
            await sleep(80);
        }

    }

    const ruffl = (sourceArray: string[], targetArray: string[]): Promise<void>[] => {
        return sourceArray.map((i, idx) => singleruffl(idx, targetArray) ); 
    };

    const windup = async () => {
        initialiseMap(firstArray); 
        await ruffl(firstArray, secondArray);
        check(secondArray); 
        currentValue.next(secondArray);
    }

    const winddown = async () => {
        initialiseMap(secondArray); 
        await ruffl(secondArray, firstArray);
        check(firstArray); 
        currentValue.next(firstArray);
    }

    const check = (targetArray: string[]) => {
        targetArray.forEach((letter, idx) => {
            
            if(map[idx] !== letter) {
                singleruffl(idx, targetArray); 
            }
            else {
                console.log("IN CHECK: ", map[idx], letter);
            }
        })
    }

    const activate = (idx: number) => {
        return colormap[currentColorIndex] && colormap[currentColorIndex].includes(idx); 
    }

</script>

<section style="grid-auto-columns: {100/firstArray.length}%" on:mouseenter={windup} on:mouseleave={winddown}>

    {#each firstArray as item, idx}
    {#if map[idx]}
    <span style="background-color: {backgroundColor};" class="block" class:active={activate(idx)}>
        <h1>{ map[idx] }</h1>
        <h1>{ $currentValue[idx] }</h1>
    </span> 
    {/if}
    {/each}
    
    <p>{$currentValue}</p>   
</section>


<style lang="scss">

    @import '../design/breakpoints.scss';

    section {
        // margin-bottom: 2px;
        display: grid;
        grid-auto-flow: column;
        height: 33%;
        position: relative; 
        // gap: 2px;
        // margin-bottom: 2px;
        // align-content: center;
        // justify-content: center;
        // justify-items: center;
    }
    section p {
        position: absolute; 
    }
    // .block {
    //     padding: 10px;
    //     width: 100%;
    //     background-color: red;
    // }
    h1 {
        font-size: 10em;
        font-weight: bold;
        width: 100%;
        text-align: center;
        margin: 0; 
    }

    .active {
        h1 {
            color: red; 
        }
    }
    

</style> -->
















<!-- 

<script lang="ts">

    import { BehaviorSubject, concat, from } from "rxjs"; 
    import { map, switchMap, startWith, pairwise, concatMap } from "rxjs/operators"; 
    export let backgroundColor = "var(--offWhite)";

    export let firstString = "";
    export let secondString = "";
    export let alphabet = "abcdefghijklmnopqurstuvwxyz123456789"; 
    export let colormap = {}; 
    export let currentColorIndex = 1; 

    const firstArray = firstString.split("");
    const secondArray = secondString.split("");
    const alphabetArray = alphabet.split("");

    let source = new BehaviorSubject(firstArray);
    let letterMap: { [key: number]: BehaviorSubject<string> } = {};
    
        firstArray.reduce((acc, curr, idx) => {
        return {
            ...letterMap,
            [idx]: new BehaviorSubject(curr)
        }
    }, letterMap); 

    let currentValue = source.pipe(
        pairwise(),
        map(([ previousArray, currentArray ]) => {
            console.log("PREVIOUS ARRAY: ", previousArray);
            return ruffl(previousArray, currentArray);
        })
    );

    const singleruffl = async (idx: number, targetArray: string[]) => {
        
        if(idx === 0) {
            console.log("STARTING SINGLERUFFL in ", secondString, letterMap); 
        };

        let runningIdx = idx; 
        let target = targetArray[idx]; 
        
        while( letterMap[ idx ] !== target ) {
            runningIdx = (runningIdx === alphabetArray.length - 1) ? 0 : (runningIdx + 1); 
            letterMap[ idx ] = alphabetArray[ runningIdx ];
            await sleep(80);
        }

    }

    function sleep(ms) {
        return new Promise( fulfil => setTimeout(fulfil, ms) )
    }

    const ruffl = (sourceArray: string[], targetArray: string[]): Promise<void>[] => {
        return sourceArray.map((i, idx) => singleruffl(idx, targetArray) ); 
    };

    const windup = async () => source.next(secondArray);

    const winddown = async () => source.next(firstArray);

    const activate = (idx: number) => {
        return colormap[currentColorIndex] && colormap[currentColorIndex].includes(idx); 
    }

</script>

<section style="grid-auto-columns: {100/firstArray.length}%" on:mouseenter={windup} on:mouseleave={winddown}>

    {#each firstArray as item, idx}
    {#if letterMap[idx] }
    <span style="background-color: {backgroundColor};" class="block" class:active={activate(idx)}>
        
        <h1>{ letterMap[idx] }</h1>
        
  

    </span> 
    {/if}
    {/each}
    
    <p>{$currentValue}</p>   
</section>


<style lang="scss">

    @import '../design/breakpoints.scss';

    section {
        // margin-bottom: 2px;
        display: grid;
        grid-auto-flow: column;
        height: 33%;
        position: relative; 
        // gap: 2px;
        // margin-bottom: 2px;
        // align-content: center;
        // justify-content: center;
        // justify-items: center;
    }
    section p {
        position: absolute; 
    }

    h1 {
        font-size: 10em;
        font-weight: bold;
        width: 100%;
        text-align: center;
        margin: 0; 
    }

    .active {
        h1 {
            color: red; 
        }
    }
    

</style>





 -->


 



 

<script lang="ts">

    import { BehaviorSubject, EMPTY, of, Observable } from 'rxjs';
    import { switchMap, expand, tap, delay } from 'rxjs/operators';

    export let backgroundColor = "var(--offWhite)";
    export let firstString = "";
    export let secondString = "";
    export let alphabet = "abcdefghijklmnopqurstuvwxyz123456789"; 
    export let colormap = {}; 
    export let currentColorIndex = 1; 

    const firstArray = firstString.split("");
    const secondArray = secondString.split("");
    const alphabetArray = alphabet.split("");


    // const singleRuffl = async (idx: number, target: string) => {
        
    //     if(idx === 0) {
    //         console.log("STARTING SINGLERUFFL in ", secondString, letterMap); 
    //     };

    //     let runningIdx = idx; 
        
    //     while( letterMap[ idx ] !== target ) {
    //         runningIdx = (runningIdx === alphabetArray.length - 1) ? 0 : (runningIdx + 1); 
    //         letterMap[ idx ] = alphabetArray[ runningIdx ];
    //         await sleep(80);
    //     }

    // }

    // const letters$ = intervalSubject.pipe(
    //     switchMap(i => {
    //             return interval(i).pipe(
    //                 scan<number, Letters>(
    //                     (letters, value, idx) => {
    //                         console.log("IN SCAN: ", value, idx);
    //                         return {
    //                             intrvl: i,
    //                             ltrs: [({
    //                             letter: randomLetter(),
    //                             yPos: Math.floor(Math.random() * gameWidth)
    //                             }), ...letters.ltrs]
    //                         }
    //                     }, 
    //                     { ltrs: [], intrvl: 0 }
    //                 )
    //             )
    //         }
    //     ),
    //     takeWhile(state => state.ltrs.length <= endThreshold)
    // );

    // const letters$ = from(firstArray).pipe(
    //     switchMap(i => i),
    //     scan<string, Letters>(
    //         (letters, value, idx) => {
    //             console.log("IN SCAN: ", value, idx);
    //             return {
    //                 intrvl: 600,
    //                 ltrs: [({
    //                 letter: value, // randomLetter(),
    //                 yPos: Math.floor(Math.random() * gameWidth)
    //                 }), ...letters.ltrs]
    //             }
    //         }, 
    //         { ltrs: [], intrvl: 0 }
    //     ),
    //     takeWhile(state => state.ltrs.length <= endThreshold),
    // );

    // const letters$ = intervalSubject.pipe(
    //     switchMap(i => {
    //         return interval(i).pipe(
    //             switchMap(i => 
    //                 from(firstArray).pipe(
    //                     switchMap(letter => letter),
    //                     scan<string, Letters>(
    //                         (letters, letter, idx) => {
    //                             console.log("IN SCAN: ", letter, idx);
    //                             return {
    //                                 intrvl: 600,
    //                                 ltrs: [({
    //                                 letter: singleRuffl(idx, letter) // letter, // randomLetter(),
    //                                 yPos: Math.floor(Math.random() * gameWidth)
    //                                 }), ...letters.ltrs]
    //                             }
    //                         }, 
    //                         { ltrs: [], intrvl: 0 }
    //                     ),
    //                     takeWhile(state => state.ltrs.length <= endThreshold),
    //                 )
    //             )
    //         )
    //     }),
    //     // takeWhile(state => state.ltrs.length <= endThreshold)
    // );


    // const arr = [ '1', '2', '3', '4', '5' ];
    const get = (idx): Observable<{ idx: number, letter: string }> => {
        currentIdx = idx; 
        return of({ idx, letter: alphabetArray[idx] });
    } 
    let target = new BehaviorSubject('y'); 
    let currentIdx = 0; 
    const currentSymbol = 
        target.pipe(
            switchMap((targetLetter: string) => {
                    return get( currentIdx ).pipe(
                    expand(({ idx, letter }) => {
                        console.log("IN EXPAND: ", idx, letter); 
                        if(idx === alphabetArray.length - 1) {
                            idx = -1;
                        }
                        return letter !== targetLetter ? get(idx + 1).pipe( delay(100) ) : EMPTY
                    }),
                    tap(v => console.log("IN TAP: ", v))
                )
            })
        )

</script>

<section >
    <span>{ target }</span>
    <h1 on:mouseenter={() => {target.next('a')}} on:mouseleave={() => {target.next('y')}}>{ $currentSymbol.letter }</h1>

    <!-- {#if $letters$ }

    {#each $letters$.ltrs.reverse() as item, idx}
    <span style="background-color: {backgroundColor};" class="block" >
        
        <h1>{ item.letter }</h1>
        
    </span> 
    {/each}

    {/if} -->
    
</section>


<style lang="scss">

    @import '../design/breakpoints.scss';

    section {
        // margin-bottom: 2px;
        display: grid;
        grid-auto-flow: column;
        height: 33%;
        position: relative; 
        // gap: 2px;
        // margin-bottom: 2px;
        // align-content: center;
        // justify-content: center;
        // justify-items: center;
    }
    section p {
        position: absolute; 
    }

    h1 {
        font-size: 10em;
        font-weight: bold;
        width: 100%;
        text-align: center;
        margin: 0; 
    }

    .active {
        h1 {
            color: red; 
        }
    }
    

</style>





