<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <button class="button">Rest </button>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import { Observable, of , pipe, fromEvent }  from 'rxjs';

import { map, scan } from 'rxjs/operators';

@Component
export default class HelloWorld extends Vue {
  @Prop() private msg!: string;
  private button!: HTMLInputElement;

  private mounted(){
    this.button = document.getElementsByClassName("button")[0] as HTMLInputElement;
    const actionObs = fromEvent(this.button, 'click');
    actionObs.pipe(
                scan(count => count + 1, 0),
              )
              .subscribe((count) =>console.log(`Clicked ${count} times`));
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
