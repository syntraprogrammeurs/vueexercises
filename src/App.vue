<script setup>
import TaskCard from "./components/TaskCard.vue"
import BoardColumn from "./components/BoardColumn.vue"

const columns = [
  {
    id: "original",
    title: "Taken (origineel)",
    color: "blue",
    tasks: [
      {
        id: 1,
        title: "Nieuwe cursusstructuur uitwerken",
        description: "Modules herzien en oefeningen toevoegen.",
        assignee: "Tom Vanhoutte",
        role: "Full stack developer",
        priority: "hoog",
        done: false
      },
      {
        id: 2,
        title: "Design voor dashboard maken",
        description: "Wireframes en kleurenpalet bepalen.",
        assignee: "Sarah Janssens",
        role: "UI designer",
        priority: "middel",
        done: false
      },
      {
        id: 3,
        title: "Component structuur bepalen",
        description: "Structuur van de componentenboom uitwerken.",
        assignee: "Noah Martens",
        role: "Software architect",
        priority: "middel",
        done: false
      }
    ]
  },
  {
    id: "byRole",
    title: "Taken (op functie)",
    color: "green",
    tasks: [
      {
        id: 4,
        title: "Nieuwe cursusstructuur uitwerken",
        description: "Modules herzien en oefeningen toevoegen.",
        assignee: "Tom Vanhoutte",
        role: "Full stack developer",
        priority: "hoog",
        done: false
      },
      {
        id: 5,
        title: "Component structuur bepalen",
        description: "Structuur van de componentenboom uitwerken.",
        assignee: "Noah Martens",
        role: "Software architect",
        priority: "middel",
        done: false
      },
      {
        id: 6,
        title: "Design voor dashboard maken",
        description: "Wireframes en kleurenpalet bepalen.",
        assignee: "Sarah Janssens",
        role: "UI designer",
        priority: "middel",
        done: false
      }
    ]
  }
]
</script>

<template>
  <main class="min-h-screen bg-slate-950 text-slate-100 px-6 py-8">
    <header class="mb-6">
      <h1 class="text-2xl font-bold">
        Mijn kanban board (statische versie)
      </h1>
      <p class="text-slate-400 text-sm mt-1">
        In deze versie staan alle kaarten nog vast in de code. Later maken we dit
        dynamisch.
      </p>
    </header>

    <section class="grid gap-6 md:grid-cols-2">
      <BoardColumn
          v-for="column in columns"
          :key="column.id"
          :title="column.title"
          :count="column.tasks.length"
          :color="column.color"
      >
        <p class="text-xs text-slate-400">
          Hier komen straks de kaarten van deze kolom.
        </p>
      </BoardColumn>
    </section>
  </main>
</template>
