# ajax
- form aciton 변경
````
$("#form-id").attr("action", "경로");
````
- submit 버튼 클릭, 함수 호출
````
$(document).ready(function () {
    $("#form-id").submit(function (event) {
        event.preventDefault();
        ajax();
    });
});
````
- ajax
````
function ajax() {
    var data = {}
    data["hello"] = $("#hello").val();
    $.ajax({
        type: "POST",
        contentType: "application/json",
        url: "주소",
        data: JSON.stringify(data),
        dataType: 'json',
        success: function (data) {
            var json = "<pre>" + JSON.stringify(data, null, 4) + "</pre>";
            $('#text-id').html(json);
        },
        error: function (e) {
            console.log("ERROR : ", e);
        }
    });
}
````