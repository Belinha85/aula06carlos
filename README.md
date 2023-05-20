# aula06carlos
aula07carlos

criar site de filme com descrição


https://www.invertexto.com/infinityaula06

/**
 * cd nome do projeto
 * rd .git /S/Q
 * git init 
 * git add . 
 * git commit -m "first commit"
 * git remote add origin <link do reposition>
 * git push -u origin master
 * 
 */

----------------------------------

https://www.invertexto.com/infinityaula06

-----------------
criar pasta .env

VITE_API_KEY=api_key=7fb6a5fdec0b9519193e4f57e16a2555
VITE_API=https://api.themoviedb.org/3/movie/
VITE_SEARCH=https://api.themoviedb.org/3/search/movie
VITE_IMG=https://image.tmdb.org/t/p/w500/

---------------------------index.css

* {
    font-family: Helvetica;
    padding: 0;
    margin: 0;
    box-sizing: border-box;
}

body {
    background-color: #000;
    color: #fff;
}

a {
    text-decoration: none;
    color: #f7d354;
    transition: .5s;
}

a:hover {
    color: #b8930c;
}
-----------------------------MovieGrid.css

.container .title {
    color: #fff;
    font-size: 2.5rem;
    text-align: center;
    margin: 2rem 0 1rem;
}

.title .query-text {
    color: #f7d354;

}

.movies-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    padding: 2rem;
    max-width: 1200px;
    margin: 0 auto;
}

.movies-container div {
    width: 30%;
    color: #fff;
    margin-bottom: 2.5rem;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    background-color: #111;
    padding: 1rem;
}

.movies-container div img {
    max-width: 100%;
}

.movies-container div img, 
.movies-container div p,
.movies-container div h2 {
    margin-bottom: 1rem;
}

.movies-container div svg {
    color: #f7d354;
}

.movies-container div a {
    background-color: #f7d354;
    border: 2px solid #f7d354;
    border-radius: 4px;
    color: #000;
    padding: 1rem 0.5rem;
    font-size: 1.3rem;
    text-align: center;
    transition: .4s;
    font-weight: bold;
}

.movies-container div a:hover {
    background-color: transparent;
    color: #f7d354;
}

-----------------------------Navbar.css
#navbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1rem 2rem;
    background-color: #121212;
}

#navbar h2 a {
    display: flex;
    align-items: center;
    gap: 0.5rem;
}

#navbar form {
    display: flex;
    gap: .5rem;
}

#navbar input {
    padding: 0.2rem 0.8rem;
    border-radius: 4px;
    border: none;
}

#navbar form button {
    background-color: #f7d354;
    border: 2px solid #f7d354;
    border-radius: 4px;
    color: #000;
    padding: 0.3rem;
    font-size: 1.3rem;
    display: flex;
    align-items: center;
    cursor: pointer;
    transition: .4s;
}

#navbar form button:hover {
    background-color: transparent;
    color: #f7d354;
}

--------------------------------------Movie.css

.movie-page {
    color: #fff;
    display: flex;
    flex-direction: column;
    max-width: 600px;
    margin: 2rem auto;
}

.movie-page svg {
    font-size: 1.5rem;
    color: #f7d354;
}

.movie-page .movie-card {
    text-align: center;
}

.movie-page .movie-card img,
.movie-page .movie-card h2,
.movie-page .movie-card p {
    margin-bottom: 1rem;
}

.movie-page .movie-card h2 {
    font-size: 2rem;
}

.movie-page .movie-card p {
    display: flex;
    align-items: center;
    justify-self: center;
    gap: 0.4rem;
}

.tagline {
    text-align: center;
    font-size: 1.3rem;
    margin-bottom: 2rem;
}

.info {
    margin-bottom: 1.5rem;
}

.info h3 {
    margin-bottom: 1rem;
    display: flex;
    align-items: center;
    gap: .5rem;
}

.description {
    padding-bottom: 10rem;
}

.description p {
    line-height: 1.5rem;
}


---------------------------- Main.jsx 

import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App.jsx';
import './index.css';

import { BrowserRouter, Routes, Route } from 'react-router-dom';

// import das páginas
import Home from './pages/Home.jsx';
import Movie from './pages/Movie.jsx';
import Search  from './pages/Search.jsx';

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <BrowserRouter>
      <Routes>
        <Route element={<App />} >
          <Route path="/" element={<Home />} />
          <Route path="/movie/:id" element={<Movie />} />
          <Route path="/search" element={<Search />} />
        </Route>
      </Routes>
    </BrowserRouter>
  </React.StrictMode>,
);

-------------------- App.jsx 
import './App.css'
import { Outlet } from 'react-router-dom'
import Navbar from './components/Navbar'
function App() {

  return (
    <div>
      <Navbar />
      <Outlet />
    </div>
  )
}

export default App


----------------- MovieCard.jsx

import { Link } from 'react-router-dom';
import { FaStar } from 'react-icons/fa';

const urlImg = import.meta.env.VITE_IMG;

const MovieCard = ({movie, showLink = true}) => {
    return (
        <div className="movie-card">
            <img src={urlImg + movie.poster_path} alt={movie.title} />
            <h2>{movie.title}</h2>
            <p>
                <FaStar /> {movie.vote_average}
            </p>
            {showLink && <Link to={`movie/${movie.id}`}>Detalhes</Link>}
        </div>
    );
};

export default MovieCard;

-----------------------------------------Home.jsx

import { useState, useEffect } from "react"
import MovieCard from '../components/MovieCard'

// import css
import './MovieGrid.css';

// importando as variaveis de ambiente (urls)
const movieURL = import.meta.env.VITE_API;
const apiKey = import.meta.env.VITE_API_KEY;

const Home = () => {
  // Criando um state para armazenar os resultados
  // da busca da API 
  const [movies, setMovies] = useState([]);

  // Função que irá buscar os filme na API
  const getMovies = async (url) => {
    const resposta = await fetch(url);
    const dados = await resposta.json();
    setMovies(dados.results);

  }

  // Chamando a função quando a página carregar
  useEffect(() => {
    // Montando a url
    const url = `${movieURL}top_rated?${apiKey}`;
    // Chamando a função
    getMovies(url);
  }, [])


  return (
    <div className="container">
      <h2 className="title">
        Melhores Filmes: 
      </h2>
      <div className="movies-container">
      {movies.map((movie) => <MovieCard key={movie.id} movie={movie} />)}
      </div>
    </div>
  )
}

export default Home

------------------- Navbar.jsx
import { useState } from "react";
import { Link, useNavigate } from 'react-router-dom';
import {BiCameraMovie, BiSearchAlt2} from 'react-icons/bi';
import './Navbar.css';

const Navbar = () => {
    const [search, setSearch] = useState("");
    // criando objeto de redirecionamento (navigate)
    const navigate = useNavigate();

    // Criando função de busca do formulário
    const handleSubmit = (e) => {
        e.preventDefault();
        // se a busca tiver vazia, para a função
        if (!search) {
            return 
        }

        navigate(`/search?filme=${search}`)
        setSearch("");

    }
  return (
    <nav id="navbar">
        <h2>
            <BiCameraMovie />
            <Link to="/">Infiniflix</Link>
        </h2>
        <form onSubmit={handleSubmit}>
            <input type="text" placeholder="Busque um filme"
            onChange={(e) => setSearch(e.target.value)}
            value={search} />
            <button type="submit">
                <BiSearchAlt2 />
            </button>
        </form>
    </nav>
  )
}

export default Navbar

---------------------------- Search.jsx 

import { useState, useEffect } from 'react';
import { useSearchParams } from 'react-router-dom';
import MovieCard from '../components/MovieCard';
import './MovieGrid.css';

// importando as variaveis de ambiente para a URL
const searchURL = import.meta.env.VITE_SEARCH;
const apiKey = import.meta.env.VITE_API_KEY;

const Search = () => {
  const [searchParams] = useSearchParams();
  // Pegando o parametro filme da url e armazenando em query
  const query = searchParams.get("filme");

  // Criando a lista que vai armazenar os filmes 
  const [movies, setMovies] = useState([]);

  const getMovies = async (url) => {
    const resposta = await fetch(url);
    const dados = await resposta.json();
    setMovies(dados.results);
  };

  useEffect(() => {

    const url = `${searchURL}?${apiKey}&query=${query}`;
    getMovies(url);

  }, [query]);

  return (
    <div className="container">
      <h2 className="title">
        Resultados para: {query} 
      </h2>
      <div className="movies-container">
      {movies.map((movie) => <MovieCard key={movie.id} movie={movie} />)}
      </div>
    </div>
  );
};

export default Search;

------------------------------ Movie.jsx

import { useState, useEffect } from 'react';
import { useParams } from 'react-router-dom';
import {
    BsGraphUp,
    BsWallet2,
    BsHourglassSplit,
    BsFillFileEarmarkTextFill
} from 'react-icons/bs';
import MovieCard from '../components/MovieCard';
import './Movie.css';

const movieURL = import.meta.env.VITE_API;
const apiKey = import.meta.env.VITE_API_KEY;

const Movie = () => {
    // Pegando o parametro id da url 
    const { id } = useParams();
    const [movie, setMovie] = useState(null);

    const getMovie = async (url) => {
        const resposta = await fetch(url);
        const dado = await resposta.json();
        setMovie(dado);
    }

    // criando useEffect para puxar o filme da API
    useEffect(() => {
        const url = `${movieURL}${id}?${apiKey}`;
        getMovie(url);
    }, []);

    return (
        <div className="movie-page">
            {
                movie && (
                    <>
                        <MovieCard movie={movie} showLink={false} />
                        <p className="tagline">{movie.tagline}</p>

                        <div className="info">
                            <h3>
                                <BsWallet2 /> Orçamento:
                            </h3>
                            <p>{movie.budget}</p>
                        </div>

                        <div className="info">
                            <h3>
                                <BsGraphUp /> Receita:
                            </h3>
                            <p>{movie.revenue}</p>
                        </div>

                        <div className="info">
                            <h3>
                                <BsHourglassSplit /> Duração:
                            </h3>
                            <p>{movie.runtime}</p>
                        </div>

                        <div className="info">
                            <h3>
                                <BsFillFileEarmarkTextFill /> Descrição:
                            </h3>
                            <p>{movie.overview}</p>
                        </div>


                    </>
                )
            }
        </div>
    );
};

export default Movie;
