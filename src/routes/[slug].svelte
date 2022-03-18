<script context="module">
  import { gql, GraphQLClient } from "graphql-request";

  export async function load({ params }) {
    const env = import.meta.env;
    const graphcms = new GraphQLClient(`${env.VITE_GRAPHCMS_URL}`, {
      headers: {}
    });

    const query = gql`
    query MyQuery {
        article(where: { slug: "${params.slug}" }) {
          id
          title
          slug
          body {
            html
          }
          photos {
            url
          }
        }
      }
    `;

    const { article } = await graphcms.request(query);

    return {
      props: {
        article: article
      }
    };
  }
</script>

<script>
  export let article;
</script>

<a class="py-2 px-4 bg-red-800 rounded text-white hover:opacity-70" href="/">
  BACK
</a>

<div class="bg-white mt-2 p-3 rounded">
  <h2 class="font-bold text-lg">{article.title}</h2>
  <hr class="mb-5" />
  <div class="text-stone-600">
    {@html article.body.html}
  </div>
  <div class="grid sm:grid-cols-2 md:grid-cols-5 gap-1">
    {#each article.photos as photo}
      <a href={photo.url} target="_blank">
        <img src={photo.url} alt="basketball" />
      </a>
    {/each}
  </div>
</div>
