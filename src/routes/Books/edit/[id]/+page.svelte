<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
    import {goto} from "$app/navigation";
    import {request,gql} from "graphql-request";
    let id = '';
    let book = {
        id: '',
        title: '',
        author: ''
    };
    let intId ;
    async function updateBook(event) {
        event.preventDefault(); // Prevent the default form submission

        try {
            await request('http://localhost:8080/query', gql` mutation{
        updateBook(id:${intId},input:{
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

    onMount(async () => {
        try {
            const url = window.location.pathname;
            console.log(url);
            id = url.split('/')[2];
            console.log(id);
            intId = parseInt(id);
            console.log(typeof id);
            // const id = '2'; // Replace with the actual book ID
            await request('http://localhost:8080/query',gql`
             query{
                book(id:${intId}){
                    id
                    title
                    year
                    author
                }
            }`)
                .then((data) => {
            book = data.book;
        });
        }
        catch (error) {
            console.error(error);
        }
    });
</script>
<h1 class="text-2xl font-bold mb-4">{book.title}</h1>
<form on:submit={updateBook}>
    <input class="p-2 border-2" placeholder="Title" bind:value={book.title} required>
    <input class="p-2 border-2" placeholder="Author" bind:value={book.author} required>
    <button type="submit" class="bg-blue-500 text-white px-4 py-2">
        Update Book
    </button>
</form>
