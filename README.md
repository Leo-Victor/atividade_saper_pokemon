# atividade_saper_pokemon

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Saper - Atividade Pok&eacute;mon</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-GLhlTQ8iRABdZLl6O3oVMWSktQOp6b7In1Zl3/Jr59b6EGGoI1aFkw7cmDA6j6gD" crossorigin="anonymous">

</head>
<body>

    
    <div class="container bg-white">

        <form class="row g-3 pt-3">
            <div class="col">
              <input type="text" readonly class="form-control-plaintext" id="staticEmail2" value="Digite o nome do Pokémon">
            </div>
            <div class="col">
              <input type="text" class="form-control" id="inputPokemon" placeholder="Mewtwo">
            </div>
            <div class="col">
              <button type="submit" class="btn btn-primary mb-3" onclick="fetchPokemon(inputPokemon.value); return false" onsubmit="return false;" >Carregar dados</button>
            </div>
          </form>

        <div class="row bg-light gx-5 rounded my-md-3 my-sm-0">
            <div class="col  text-dark">
                <h1 class="h2 text-center name">????</h1>
            </div>
        </div>

        <div class="row rounded g-0 gx-5">
            <div class="col-sm-12 col-md-6 bg-light rounded shadow-sm">
                <div class="text-center">
                    <img src="" class="mx-auto d-block w-50" alt="Deveria aparecer um Pokémon aqui." id="sprite">
                  </div>
            </div>

            <div class="col-sm-12 col-md-6 bg-light d-flex align-items-center rounded shadow-sm">

                <ul class="list-group list-group-flush w-100">
                    <li class="list-group-item d-flex justify-content-between align-items-center bg-light">
                        HP:
                        <span id="hp">??</span>
                    </li>
                    <li class="list-group-item d-flex justify-content-between align-items-center bg-light">
                        Attack:
                        <span id="atc">??</span>
                    </li>
                    <li class="list-group-item d-flex justify-content-between align-items-center bg-light">
                        Defense:
                        <span id="def">??</span>
                    </li>
                    <li class="list-group-item d-flex justify-content-between align-items-center bg-light">
                        Special Attack:
                        <span id="spa">??</span>
                    </li>
                    <li class="list-group-item d-flex justify-content-between align-items-center bg-light">
                        Special Defense:
                        <span id="spd">??</span>
                    </li>
                    <li class="list-group-item d-flex justify-content-between align-items-center bg-light">
                        Speed:
                        <span id="spe">??</span>
                    </li>
                  </ul>


            </div>
        </div>


    </div>


    <script type="text/javascript">

        qS = (selector) => document.querySelector(selector)

        function setText(selector, text) {
            let element = document.querySelector(selector);
            element.innerHTML = text;
        }

        function setName({name}) {
            if(!name) {
                qS('.name').innerHTML = '&nbsp;';
                return;
            }

            qS('.name').innerHTML =  name.toUpperCase();

        }

        function setSprite( {sprites: {front_default: sprite}} ) {
            if(!sprite) {
                qS('img').src = '';
                qS('img').classList.add('placeholder');    
            } else {
                qS('img').classList.remove('placeholder');    
                qS('img').src = sprite;
            }
        }

        function setStats({stats: {0:hp, 1:atc, 2:def, 3:spa, 4:spd, 5:spe}}) {
            // TODO com a sintaxe que desestrutura o objeto e preenche o resto da tela
            qS('#hp').textContent = hp.base_stat;
            qS('#atc').textContent = atc.base_stat;
            qS('#def').textContent = def.base_stat;
            qS('#spa').textContent = spa.base_stat;
            qS('#spd').textContent = spd.base_stat;
            qS('#spe').textContent = spe.base_stat;
        }

        function cleanPokemonData() {
            setPokemonData( {sprites: undefined, name: '', stats: undefined} );
        }

        function setPokemonData( pokemonJson ) {
            setName(pokemonJson);
            setSprite(pokemonJson);
            setStats(pokemonJson);

            return false;
        }


        const fetchPokeApi = (url, fn) =>  
            fetch(url)
                .then( (resp) => resp.json() )
                .then( fn )
                .catch( (err) => { cleanPokemonData(); alert(err); } );
        
        const fetchPokemon = (name) => fetchPokeApi(
            `https://pokeapi.co/api/v2/pokemon/${name}`,
            setPokemonData
        );



        window.addEventListener('load', (event) => {
            fetchPokemon('pikachu');
        });
    </script>
</body>
</html>
