[index.html](https://github.com/user-attachments/files/28265308/index.html)
const API_KEY = "본인_API_KEY";
const STATION_NAME = "부천종합운동장";

async function getSubwayArrival() {

```
const url =
`https://swopenAPI.seoul.go.kr/api/subway/${API_KEY}/json/realtimeStationArrival/0/5/${STATION_NAME}`;

try {
    const response = await fetch(url);
    const data = await response.json();

    document.getElementById('station-name').innerText =
        STATION_NAME + '역';

    const listContainer =
        document.getElementById('arrival-list');

    listContainer.innerHTML = '';

    if (data.realtimeArrivalList) {

        const line7Data =
            data.realtimeArrivalList.filter(
                item => item.subwayId === "1007"
            );

        if (line7Data.length === 0) {
            listContainer.innerHTML =
                '<div class="arrival-item">운행 정보 없음</div>';
            return;
        }

        line7Data.forEach(item => {

            let arrivalMsg = item.arvlMsg2;

            let isImminent =
                arrivalMsg.includes("진입") ||
                arrivalMsg.includes("도착") ||
                arrivalMsg.includes("전역");

            const itemDiv =
                document.createElement('div');

            itemDiv.className = 'arrival-item';

            itemDiv.innerHTML = `
                <div class="dir">
                    🚇 ${item.trainLineNm}
                </div>

                <div class="time ${isImminent ? 'imminent' : ''}">
                    ${arrivalMsg}
                </div>
            `;

            listContainer.appendChild(itemDiv);
        });
    }

    const now = new Date();

    document.getElementById('update-time').innerText =
        `최근 업데이트: ${now.toLocaleTimeString()}`;

} catch (error) {

    document.getElementById('arrival-list').innerHTML =
        '<div class="arrival-item" style="color:red;">로드 실패</div>';
}
```

}

getSubwayArrival();

setInterval(getSubwayArrival, 30000);
