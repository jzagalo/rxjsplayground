<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <button class="button">Rest </button>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import { Observable, of , pipe, fromEvent, observable,
        interval, Subject, from, merge  }  from 'rxjs';

import { map, scan, throttle, throttleTime, multicast } from 'rxjs/operators';

@Component
export default class HelloWorld extends Vue {
  @Prop() private msg!: string;
  private button!: HTMLInputElement;

  private mounted(){
    this.button = document.getElementsByClassName("button")[0] as HTMLInputElement;
    const actionObs = fromEvent(this.button, 'click');
    const subjectObs = Observable.create((observer: any) => {
        //observer.next(1);
       // observer.next(2);
        //observer.next(3);

        setTimeout(() => {
          observer.next(4);
          observer.complete();
        }, 1000);
    });

    console.log('just before subscribe');
    subjectObs.subscribe({
      next: (x: number) => console.log('got value ' + x),
      error: (err: any) => console.error('something wrong occured: ' + err),
      complete: () => console.log('done')
    })

    const obs1 = interval(400);
    const obs2 = interval(300);

    const sub1 = obs1.subscribe(x => console.log('first: ' + x));
    const childSub1 = obs2.subscribe(x => console.log('second: ' + x));

    sub1.add(childSub1);

    setTimeout(() => {
        sub1.unsubscribe();
    }, 1000);

    const subject = new Subject();
    subject.subscribe({
      next: (v: any) => console.log('observerA111: '+ v),
    });
    subject.subscribe({
      next: (v: any) => console.log('observerB111: '+ v),
    });

    subject.next(1);
    subject.next(2);

    const observable1 = interval(1000);
    const observable2 = interval(400);

    const merged = merge(observable1, observable2);
    const mergedSub = merged.subscribe(x => console.log(x + " Q "));
    
    setTimeout(() => {
        mergedSub.unsubscribe();
    }, 1000)


    /* actionObs.pipe(
                throttleTime(1000),
                scan(count => count + 1, 0),
              )
              .subscribe((count) =>console.log(`Clicked ${count} times`)); */
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
