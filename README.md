# OpenGLESTransformationsDemo

#Learning shader programming:

precision mediump float;
varying vec2 textureCoordinate;
uniform lowp sampler2D inputImageTexture1;


vec2 toPolar(vec2 point) {
    float r = length(point);
    float theta = atan(point.y,point.x);           //这其实就是atan2，返回的是-pi到pi
    return vec2(r, theta);
}

void main() {
    float x=textureCoordinate.x,y=textureCoordinate.y;
    if (x < 0.5) {
      x = 2.0 * x;
     } else {
      x = 2.0 * x  - 1.0;
     }

    vec2 polarBase=toPolar(vec2(-0.375,0.85*3.0));                           //矩形扇形坐标变换
    vec2 polarCur=toPolar(vec2(x-0.5,y+0.85*3.0));
    float baseR=polarBase.x,baseTheta=polarBase.y,curR=polarCur.x,curTheta=polarCur.y;
    highp vec2 p2=vec2((baseTheta-curTheta)/(2.0*baseTheta-3.1415926),3.0*(curR/baseR-1.0));
    if (all(greaterThanEqual(p2, vec2(0.0)))&&all(lessThanEqual(p2, vec2(1.0)))){
        x = p2.x;
        y = p2.y;
    } else{
        gl_FragColor = vec4(0.0, 0.0, 0.0, 1.0);
        return;
    }

    //gl_FragColor = texture2D(inputImageTexture1, vec2(x,y));
    mediump vec4 color = texture2D(inputImageTexture1, vec2(x,y));
    //gl_FragColor = vec4((1.0 - color.rgb), color.w);
    gl_FragColor = vec4(0.0-color.r,1.2-color.g,0.6-color.b, color.w);
}
