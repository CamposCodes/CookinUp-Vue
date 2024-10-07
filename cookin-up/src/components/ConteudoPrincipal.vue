<script lang="ts">
    import SelecionarIngredientes from "./SelecionarIngredientes.vue";
    import MostrarReceitas from "./MostrarReceitas.vue";
    import SuaLista from "./SuaLista.vue";
    import Tag from "./Tag.vue";

    type Pagina = "SelecionarIngredientes" | "MostrarReceitas";

    export default {
        data() {
            return {
                ingredientes: [] as string[],
                conteudo: "SelecionarIngredientes" as Pagina,
            };
        },
        components: { SelecionarIngredientes, Tag, SuaLista, MostrarReceitas },
        methods: {
            adicionarIngredientes(ingredientes: string) {
                this.ingredientes.push(ingredientes);
            },
            removerIngredientes(ingrediente: string) {
                this.ingredientes = this.ingredientes.filter(
                    (iLista) => ingrediente !== iLista
                );
            },
            navegar(pagina: Pagina) {
                this.conteudo = pagina;
            },
        },
    };
</script>

<template>
    <main class="conteudo-principal">
        <SuaLista :ingredientes="ingredientes" />
        <!-- KEEPALIVE : preserva estado dos componentes que estao dentro da tag (joga
        estado dos componmenets em cache) -->
        <KeepAlive include="SelecionarIngredientes">
            <SelecionarIngredientes
                v-if="conteudo === 'SelecionarIngredientes'"
                @adicionar-ingrediente="adicionarIngredientes"
                @remover-ingrediente="removerIngredientes"
                @buscar-receitas="navegar('MostrarReceitas')"
            />
            <MostrarReceitas
                :ingredientes="ingredientes"
                v-else-if="conteudo === 'MostrarReceitas'"
                @editar-receitas="navegar('SelecionarIngredientes')"
            />
        </KeepAlive>
    </main>
</template>

<style scoped>
    .conteudo-principal {
        padding: 6.5rem 7.5rem;
        border-radius: 3.75rem 3.75rem 0rem 0rem;
        background: var(--creme, #fffaf3);
        color: var(--cinza, #444);

        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 5rem;
    }

    @media only screen and (max-width: 1300px) {
        .conteudo-principal {
            padding: 5rem 3.75rem;
            gap: 3.5rem;
        }
    }

    @media only screen and (max-width: 767px) {
        .conteudo-principal {
            padding: 4rem 1.5rem;
            gap: 4rem;
        }
    }
</style>
