# Angular PIPE Orden por nombre o campo (orden ascendente o descendente)
Creado con el objetivo de ordenar una lista por campo mediante el uso de pipes en *ngFor

## Creamos un nuevo archivo typeScript (.ts) para el nuevo pipe con CLI:
ng g pipe pipes/orderby

## Con esto se incorpora a nuesto .module.ts automáticacmente :
```
import { OrderbyPipe } from './pipes/orderby.pipe';

@NgModule({
  declarations: [
    AppComponent,
    OrderbyPipe
  ],
})
```

## Dentro del archivo agregamos lo siguiente:

```
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'orderby'
})
export class OrderbyPipe implements PipeTransform {

  transform(array: any[], field: string, order: 'asc' | 'desc' = 'asc'): any[] {
    if ( !array || !field) {
      return array;
    }
    const sortOrder = order === 'asc' ? 1 : -1;

    return array.sort((a, b) => {
      const valueA = a[field];
      const valueB = b[field];
      return sortOrder * valueA.localeCompare(valueB, undefined, { sensitivity: 'base' });
    });
  }

}
```
## En nuestro componente.ts donde se utilizará el pipe agregamos

```
order: 'asc' | 'desc' = 'asc';
toggleOrder(): void {
    this.order = this.order === 'asc' ? 'desc' : 'asc';
  }
```
## Para finalizar agregamos a nuestro componente.html 

```
<thead>
      <tr class="table-danger">
        <th scope="col" (click)="toggleOrder()">Nombre</th>
        <th scope="col">Detalles</th>
      </tr>
    </thead>
```
```
<tr
        *ngFor="
          let item of items
            | orderby : 'name' : order;
          let i = index
        "
      >
```
