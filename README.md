# -Database-OpenAPI
#오픈API 활용

<img src="https://user-images.githubusercontent.com/72116811/206854027-2e928715-d0b9-4fbb-a749-f18f9766f627.mp4">
\~\~~
<!DOCTYPE html>
<html lang="ko">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>API 활용</title>
</head>

<body>
    <h1>국가 입국정보 및 안전정보 조회</h1>
    <input id="info" value="" type="text">
    <button id="search">검색</button>
    <h2 id="nation"> </h2>
    <div id="entrance">
        <h3> 입국정보</h3>
        <ul id="dplmt_pspt_visa_yn">일반여권 허가 여부: </ul>
        <ul id="dplmt_pspt_visa_cn">일반여권 허가 내용: </ul>
        <ul id="nvisa_entry_evdc_cn">무비자: </ul>
        <ul id="remark">비고: </ul>
    </div>
    <div id="safetyinfo">
        <h3>안전 정보</h3>
        <p id="infotxt"></p>     
    </div>

    <script src="https://code.jquery.com/jquery-3.4.1.js"
        integrity="sha256-WpOohJOqMqqyKL9FccASB9O0KwACQJpFTUBLTYOVvVU=" crossorigin="anonymous"></script>
    <script>
        $(document).ready(function () {
            $("#search").click(function () {
                $.ajax({
                    method: "GET",
                    url: "http://apis.data.go.kr/1262000/EntranceVisaService2/getEntranceVisaList2?",
                    data: {
                        serviceKey: "0sPttmY/DoHrx4UdB4UwCGXzfziG+h7QAu+YZdDnnMW0DQi63hj6bJ8wIwBVG94eettfomhY4J17D4UebJ02GQ==",
                        returnType: 'JSON',
                        numOfRows: '10',
                        pageNo: '1',
                        'cond[country_nm::EQ]': $("#info").val()
                    },
                })
                    .done(function (msg) {
                        $("#nation").append("<strong>" + msg.data[0].country_eng_nm + "</strong>" + ",");
                        $("#nation").append("<strong>" + msg.data[0].country_nm + "</strong>");
                        $("#dplmt_pspt_visa_cn").append(msg.data[0].dplmt_pspt_visa_cn);
                        $("#dplmt_pspt_visa_yn").append(msg.data[0].dplmt_pspt_visa_yn);
                        $("#nvisa_entry_evdc_cn").append(msg.data[0].nvisa_entry_evdc_cn);
                        $("#remark").append(msg.data[0].remark);
                    });
                $.ajax({
                    method: "GET",
                    url: "http://apis.data.go.kr/B260003/LocalSafetyInformationService/getLocalSafetyInformationList",
                    data: {
                        serviceKey: "0sPttmY/DoHrx4UdB4UwCGXzfziG+h7QAu+YZdDnnMW0DQi63hj6bJ8wIwBVG94eettfomhY4J17D4UebJ02GQ==",
                        numOfRows: '10',
                        pageNo: '1',
                        'cond[country_nm::EQ]': $("#info").val(),
                        'cond[type::LIKE]': '안전관리',
                    },
                })
                    .done(function (msg) {
                        var str = msg.data[0].cn
                        str = str.replace(/(?:\r\n|\r|\n)/g, '<br />');
                        $("#infotxt").html(str);
                    });
            });
        });
    </script>
</body>
</html>
\~\~~

