<script>
const { Parser } = require('json2csv')

export default {
    data() {
        return {
            fields: ['name', 'team', 'position'],
            players: [
                {
                    name: 'Stephen Curry',
                    team: 'Golden State Warriors',
                    position: 'Guard'
                },
                {
                    name: 'Damian Lillard',
                    team: 'Portland Trail Blazers',
                    position: 'Guard'
                },
                {
                    name: 'Kawhi Leonard',
                    team: 'Toronto Raptors',
                    position: 'Forward'
                },
                {
                    name: 'Brook Lopez',
                    team: 'Milwaukee Bucks',
                    position: 'Center'
                }
            ]
        }
    },
    methods: {
        returnCSV() {
            const fields = this.fields
            const players = this.players
            const parser = new Parser({ fields })
            const csv = parser.parse(players)
            // console.log(csv)
            return csv
        },
        downloadCSV(args) {
            // console.log(this.returnCSV())
            let data, filename, link
            let csv = this.returnCSV()
            if (csv == null) return
            filename = args.filename || 'export.csv'
            if (!csv.match(/^data:text\/csv/i)) {
                csv = 'data:text/csv;charset=utf-8,' + csv
            }
            data = encodeURI(csv)
            link = document.getElementById('download-link')
            link.setAttribute('href', data)
            link.setAttribute('download', filename)
            // link.click()
        }
    }
}
</script>

<template>
    <div>
        <table>
            <tr>
                <td>Name</td>
                <td>Team</td>
                <td>Position</td>
            </tr>
            <tr v-for="(player, index) in players"
                :key="index">
                <td>{{ player.name }}</td>
                <td>{{ player.team }}</td>
                <td>{{ player.position }}</td>
            </tr>
        </table>
        <br>
        <a id="download-link"
            href="#"
            @click="downloadCSV({ filename: 'players.csv' })">
            Download List of Players
        </a>
    </div>
</template>

<style lang="css" scoped>
    table, td {
        border: 1px solid black;
    }
</style>
