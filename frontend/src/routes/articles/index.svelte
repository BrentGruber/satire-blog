<script context="module">
        import ApolloClient, { gql } from 'apollo-boost';  
        import moment from 'moment';

        const blogQuery = gql`
        query Blogs {  
                blogs {
                        id
                        Title
                        Description
                        Published
                        Body
                        author {
                                username
                        }
                        Slug
                }
        }
        `;
        export async function preload({params, query}) {
                const client = new ApolloClient({ 
                        uri: 'http://localhost:1337/graphql',
                        fetch: this.fetch
                         });
                const results = await client.query({
                        query: blogQuery
                })
                return {posts: results.data.blogs}
        }
</script>

<script>
        export let posts;
</script>

<style>
        ul, p {
                margin: 0 0 1em 0;
                line-height: 1.5;
        }
        .main-title {
                font-size: 25px;
        }
</style>

<svelte:head>
        <title>articles</title>
</svelte:head>

<h1>recent posts</h1>

<ul>
{#each posts as post}
    <li>
          <a class="main-title" rel='prefetch' href='articles/{post.Slug}'>
            {post.Title}
          </a>
    </li>
    <p> 
  {moment().to(post.Published, "DD-MM-YYYY")} ago by {post.author.username} 
    </p>
{/each}
</ul>