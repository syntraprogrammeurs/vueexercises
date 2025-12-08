<script setup>
import TaskCard from "./components/TaskCard.vue"
import BoardColumn from "./components/BoardColumn.vue"
</script>

<template>
  <main class="min-h-screen bg-gray-900 text-white px-6 py-8">
    <header class="mb-6">
      <h1 class="text-2xl font-bold">
        Mijn kanban board (statische versie)
      </h1>
      <p class="text-gray-400 text-sm mt-1">
        In deze versie staan alle kaarten nog vast in de code. Later maken we dit dynamisch.
      </p>
    </header>

    <section class="flex gap-6 overflow-x-auto pb-4">
      <BoardColumn title="To Do" :count="2">
        <TaskCard
            title="Nieuwe cursusstructuur uitwerken"
            description="Modules herzien en oefeningen toevoegen."
            assignee="Tom Vanhoutte"
            role="Full stack developer"
        />
        <TaskCard
            title="Design voor dashboard maken"
            description="Wireframes en kleurenpalet bepalen."
            assignee="Sarah Janssens"
            role="UI designer"
        />
      </BoardColumn>

      <BoardColumn title="Doing" :count="1">
        <TaskCard
            title="API koppeling testen"
            description="Controleer foutafhandeling en timeouts."
            assignee="Liam De Smet"
            role="Backend developer"
        />
      </BoardColumn>

      <BoardColumn title="Done" :count="3">
        <TaskCard
            title="Vite project opgezet"
            description="Basisconfiguratie met Vue en Tailwind."
            assignee="Emma Vermeersch"
            role="Frontend developer"
        />
        <TaskCard
            title="Component structuur bepaald"
            description=""
            assignee="Noah Martens"
            role="Software architect"
        />
        <TaskCard
            title="Tailwind integratie getest"
            description=""
            assignee="Mila Peeters"
            role="Frontend developer"
        />
      </BoardColumn>
    </section>
  </main>
</template>
