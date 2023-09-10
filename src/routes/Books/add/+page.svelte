<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
    import {goto} from "$app/navigation";
    import {gql, request} from "graphql-request";

    let book = {
        id:0,
        title: '',
        author: '',
        year:''
    };

    async function addBook() {
        try {
            await request('http://localhost:8080/query', gql` mutation{
        createBook(input:{
            title:"${book.title}"
            year:"${book.year}"
            author:"${book.author}"
        }
        ){
                title
                author
                year
            }
        }`).then(data => {
                console.log(data);
                goto('/');
            });
        } catch (error) {
            console.error(error);
        }
    }

</script>

    <h1 class="text-2xl font-bold mb-4">Books</h1>

<form on:submit|preventDefault={addBook}>
    <input class="p-2 border-2" placeholder="Title" bind:value={book.title}  required>
    <input class="p-2 border-2" placeholder="Author" bind:value={book.author}  required>
    <input class="p-2 border-2" placeholder="Year" bind:value={book.year}  required>
    <button type="submit" class="bg-blue-500 text-white px-4 py-2">
        Upload Book
    </button>
</form>
