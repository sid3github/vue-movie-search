<template>
    <div class="card">
        <img
            :src="movie.Poster"
            @error="setAltImg"
            :alt="movie.Title + ' Poster'"
        />
        <h3>
            {{ movie.Title }} <span>{{ movie.Year }}</span>
        </h3>
    </div>
</template>

<script>
export default {
    props: ["movie"],
    setup() {
        const setAltImg = (event) => {
            event.target.src =
                "https://www.freeiconspng.com/uploads/no-image-icon-15.png";
        };
        return {
            setAltImg,
        };
    },
};
</script>

<style>
.card {
    flex: 1 1 33%;
    width: 33%;
    max-width: 33%;
    padding: 0 8px;
    margin-bottom: 16px;
}

img {
    width: 100%;
    height: 300px;
    object-fit: cover;
    border-radius: 16px;
    box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.15);
    transition: 0.4s;
}

h3 {
    padding: 16px;
    color: #313131;
    transition: 0.4s;
}

img:hover {
    transform: scale(1.05);
}

h3:hover {
    color: #24c444;
}

h3 > span {
    display: block;
    color: #a0a0a0;
    font-size: 14px;
    font-weight: 400;
}

@media screen and (max-width: 688px) {
    .card {
        flex: unset;
        width: unset;
        max-width: unset;
    }
}
</style>