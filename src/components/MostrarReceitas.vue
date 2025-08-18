<script lang="ts">
    import { obterReceitas } from "@/http";
    import type IReceita from "@/interfaces/IReceita";
    import BotaoPrincipal from "./BotaoPrincipal.vue";
    import CardReceita from "./CardReceita.vue";
    import type { PropType } from "vue";
import { itensDeListaEstaoEmLista2 } from "@/operações/listas";

    export default {
        props: {
            ingredientes: { type: Array as PropType<string[]>, required: true },
        },
        data() {
            return {
                receitasEncontradas: [] as IReceita[],
            };
        },
        async created() {
            const receitas = await obterReceitas();

            // Normalização para comparar strings com/sem acentos, espaços e caixa
            const normalizar = (s: string) =>
                s
                    .normalize("NFD")
                    .replace(/\p{Diacritic}/gu, "")
                    .trim()
                    .toLowerCase();

            // Ingredientes considerados "básicos" que não precisam ser selecionados (ajuste se quiser)
            const BASICOS = ["sal", "pimenta do reino", "agua", "água", "pimenta", "oleo", "óleo"];

            const selecionadosNorm = this.ingredientes.map(normalizar);

            this.receitasEncontradas = receitas.filter((receita) => {
                const receitaNorm = receita.ingredientes.map(normalizar);

                // Remove básicos da verificação de cobertura (a receita pode tê-los mesmo se não selecionados)
                const receitaFiltrada = receitaNorm.filter(
                    (ing) => !BASICOS.includes(ing)
                );

                // Se nenhum ingrediente foi selecionado, não retorna nada (evita listar tudo em branco)
                if (!selecionadosNorm.length) return false;

                // Regra desejada: TODA seleção deve estar contida na receita (receita pode ter mais coisas)
                const contemSelecaoCompleta = selecionadosNorm.every((sel) =>
                    receitaFiltrada.includes(sel)
                );

                return contemSelecaoCompleta;
            });
        },
        components: { BotaoPrincipal, CardReceita },
        emits: ["editarReceitas"],
    };
</script>

<template>
    <section class="mostrar-receitas">
        <h1 class="cabecalho titulo-receitas">Receitas</h1>

        <p class="paragrafo-lg resultados-encontrados">
            Resultados encontrados: {{ receitasEncontradas.length }}
        </p>

        <div v-if="receitasEncontradas.length" class="receitas-wrapper">
            <p class="paragrafo-lg informacoes">
                Veja as opções de receitas que encontramos com os ingredientes
                que você tem por aí!
            </p>

            <ul class="receitas">
                <li v-for="receita of receitasEncontradas" :key="receita.nome">
                    <CardReceita :receita="receita" />
                </li>
            </ul>
        </div>

        <div v-else class="receitas-nao-encontradas">
            <p class="paragrafo-lg receitas-nao-encontradas__info">
                Ops, não encontramos resultados para sua combinação. Vamos
                tentar de novo?
            </p>

            <img
                src="@/assets/images/sem-receitas.png"
                alt="Desenho de um ovo quebrado. A gema tem um rosto com uma expressão triste."
            />
        </div>

        <BotaoPrincipal texto="Editar lista" @click="$emit('editarReceitas')" />
    </section>
</template>

<style scoped>
    .mostrar-receitas {
        display: flex;
        flex-direction: column;
        align-items: center;
        text-align: center;
    }

    .titulo-receitas {
        color: var(--verde-medio, #3d6d4a);
        margin-bottom: 1.5rem;
    }

    .resultados-encontrados {
        color: var(--verde-medio, #3d6d4a);
        margin-bottom: 0.5rem;
    }

    .receitas-wrapper {
        margin-bottom: 3.5rem;
    }

    .informacoes {
        margin-bottom: 2rem;
    }

    .receitas {
        display: flex;
        justify-content: center;
        gap: 1.5rem;
        flex-wrap: wrap;
    }

    .receitas-nao-encontradas {
        margin-bottom: 2rem;
    }

    .receitas-nao-encontradas__info {
        margin-bottom: 0.5rem;
    }

    @media only screen and (max-width: 767px) {
        .receitas-wrapper {
            margin-bottom: 2rem;
        }
    }
</style>
