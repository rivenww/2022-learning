# New Module

## Why new module?

The goal of the project is to implement a top3 list via table. I originally intended to use a component class for implementation, but the assignment requires that both router and component should be implemented in the new module.

## Submodule of the root module

Subcomponents and their routing configuration will be centralized in submodules, given the assignment requirements.

```
ng g m top-heroes --routing
```

Generate a submodule named top-heroes with routing module. Then import it to _app.module_ as a child.

Next, I can implement the component and configure the routing in the new module.

## Got Top3 Heroes

```
/** GET top3 heroes from the server */
  getTopHeroes(): Observable<Hero[]> {
    return this.http.get<Hero[]>(`${this.heroesUrl}`)
      .pipe(
        map(heroes => heroes.slice(-3)),
        tap(_ => this.log('fetched top3 heroes')),
        catchError(this.handleError<Hero[]>('getTopHeroes', []))
      );
  }
```

## Table with NgFor

```
<h2>Top3 Heroes</h2>

<table id="top-list">
  <tr>
    <th>id</th>
    <th>name</th>
  </tr>
  <tr *ngFor="let hero of topHeroes" class="heroes-row">
    <td>{{hero.id}}</td>
    <td>{{hero.name}}</td>
  </tr>
</table>
```

The assignment doesn't look so complicated. So I use this project as part of a front-end and back-end separation project. Just remove the mock data module and implement its backend using Springboot and DynamoDB.
