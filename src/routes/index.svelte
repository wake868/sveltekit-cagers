<script context="module">
  import { gql, GraphQLClient } from "graphql-request";

  export async function load() {
    const env = import.meta.env;

    const graphcms = new GraphQLClient(`${env.VITE_GRAPHCMS_URL}`, {
      headers: {}
    });

    const query = gql`
      query AllArticles {
        articles(orderBy: rank_ASC) {
          id
          slug
          title
          blurb
        }
      }
    `;

    const { articles } = await graphcms.request(query);
    return {
      props: {
        articles
      }
    };
  }
</script>

<script>
  export let articles;
</script>

<div class="grid grid-cols-4 gap-2">
  {#each articles as article}
    <div class="bg-white rounded p-1 relative h-40">
      <h3 class="font-semibold">{article.title}</h3>
      <hr />
      <p class="text-stone-500 text-xs">{article.blurb}</p>
      <a
        class="py-0 px-1 bg-red-800 rounded text-white absolute bottom-1 right-1 hover:opacity-70"
        href="/{article.slug}"
      >
        VIEW
      </a>
    </div>
  {/each}
</div>
