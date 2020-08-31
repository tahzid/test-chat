 data: {
        messages: []
    },

    created() {
        this.fetchMessages();
    },

    methods: {
        fetchMessages() {
            axios.get("/messages").then(response => {
                this.messages = response.data;
            });
        },


    }
