### Node server

## Migrations

Example code 

````ts
import Knex from 'knex';

export async function up(knex: Knex){
  return knex.schema.createTable('myTable', table => {
    table.increments('id').primary();
    table.string('name').notNullable();
    table.decimal('latitude');

    table.string('otherTable_id')
    .notNullable()
    .references('id')
    .inTable('otherTable');
  });
}

export async function down(knex: Knex){
  return knex.schema.dropTable('myTable');
}

````

Run 

````Shell
npm run knex:migrate
````

## Seeds

Example code

````ts
import Knex from 'knex';

export async function seed(knex: Knex){
  await knex('items').insert([
    { title: 'Lampadas', image: 'lampadas.svg' },
    { title: 'Pilhas e baterias', image: 'baterias.svg' },
    { title: 'Paneis e Papelao', image: 'papeis-papelao.svg' },
    { title: 'Residuos Eletronicos', image: 'eletronicos.svg' },
    { title: 'Residuos Organicos', image: 'organicos.svg' },
    { title: 'Oleo de cozinha', image: 'oleo.svg' },
  ])
}

````

Run


````Shell
npm run knex:seeds
````

## Controllers

Example code

````ts
import { Request, Response } from 'express';

import knex from '../database/connection';

class ItemsController {
  async index(request: Request, response: Response){
    const items = await knex('items').select('*');
 
    const serializedItems = items.map(item => {
      return {
        name: item.title,
        image_url: `http://localhost:3333/uploads/${item.image}`,
        id: item.id
      }
    })

    return response.json(serializedItems);
  }

  async show(request: Request, response: Response){
    const id = request.params.id

    const item = await knex('items').where('id', id).first();

    if(!item) {
      return response.status(400).json({ message: "Not Found" });
    } else {
      return response.json(item);
    }

  }
}

export default ItemsController;

````

