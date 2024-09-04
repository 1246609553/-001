<?php
class Main
{
    public $weight_num = 250;
    public $in_cm = 2.54;
    public $lb_kg = 0.454;
    public function test(float $length, float $width, float $height, float $weight): array
    {
        //单位换算--》in
        $length_in = ceil($length/$this->in_cm);
        $width_in = ceil($weight/$this->in_cm);
        $height_in = ceil($height/$this->in_cm);
        $len_with_heght_in = [$length_in,$width_in,$height_in];
        sort($len_with_heght_in);
        $type_name = [];
        //长，宽，高，（升序排序）
        $len_with_heght = [$length,$width,$height];
        sort($len_with_heght);
        //围长
        $girth = ceil(($len_with_heght[2]+($len_with_heght[1]+$len_with_heght[0]) * 2)/$this->in_cm);
        //单位换算成in(向上取整)
        //体积重计算
        $vol_weight = ceil($len_with_heght_in[2]*$len_with_heght_in[1]*$len_with_heght_in[0]/$this->weight_num);
        //重量单位换算lb
        $in_weight = ceil($weight/$this->lb_kg);
        //实重
        $true_weight = max($in_weight,$vol_weight);
        //判断类型
        if($this->check_out_space($true_weight,$len_with_heght_in[2],$girth)){
            return ["OUT_SPACE"];
           
        }
        if($this->check_oversize($len_with_heght_in[2],$girth)){
            return ["OVERSIZE"];
        }
        if($this->check_ahs_weight($true_weight)){
            $type_name[] =  ["AHS-WEIGHT"];
        }
        if($this->check_ahs_siz($girth,$len_with_heght_in[2],$len_with_heght_in[1])){
            $type_name[] =  ["AHS-SIZE"];
        }
        return $type_name;
    }
    //类型定义判断    
    public function check_out_space(float $true_weight,float $len_with_heght_max,float $girth): bool
    {
        $boolen = false;
        if (($true_weight > 150) || ($len_with_heght_max >= 108) || ($girth > 165)){
            $boolen = true;
        }
        return $boolen;
    } 
    public function check_oversize(float $len_with_heght_max,float $girth): bool
    {
        $boolen = false;
        if(($girth > 130 && $girth <=165) || ($len_with_heght_max >= 96 && $len_with_heght_max <108)){
            $boolen = true;
        }
        return $boolen;
    }
    public function check_ahs_weight(float $true_weight): bool
    {
        $boolen = false;
        if($true_weight >50 && $true_weight <=150){
            $boolen = true; 
        }
        return $boolen;
    }
    public function check_ahs_siz(float $girth, float $len_with_heght_max, float $len_with_heght_s): bool
    {
        $boolen = false;
        if(($girth >105) || ($len_with_heght_max >= 48 && $len_with_heght_max < 108) || $len_with_heght_s >= 30){
            $boolen = true; 
        }
        return $boolen;
    }

}

$obj = new Main();
var_dump($obj->test(68, 70, 60, 23));
echo "</br>";
var_dump($obj->test(114.50,42,26,47.5));
echo "</br>";
var_dump($obj->test(162, 60, 11, 14));
echo "</br>";
var_dump($obj->test(113, 64, 42.5, 35.85));
echo "</br>";
var_dump($obj->test(114.5, 17, 51.5, 16.5));
?>
