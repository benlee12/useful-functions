# useful-functions
rotate and crop

def rotate_image(img, angle, crop):
    h, w = img.shape[:2]
    angle %= 360
    M_rotation = cv2.getRotationMatrix2D((w / 2, h / 2), angle, 1)
    img_rotated = cv2.warpAffine(img, M_rotation, (w, h))
    # 除黑边
    if crop:
        angle_crop = angle % 180
        if angle > 90:
            angle_crop = 180 - angle_crop
        theta = angle_crop * np.pi / 180
        hw_ratio = float(h) / float(w)
        tan_theta = np.tan(theta)
        numerator = np.cos(theta) + np.sin(theta) * np.tan(theta)
        r = hw_ratio if h > w else 1 / hw_ratio
        denominator = r * tan_theta + 1
        crop_mult = numerator / denominator
        w_crop = int(crop_mult * w)
        h_crop = int(crop_mult * h)
        x0 = int((w - w_crop) / 2)
        y0 = int((h - h_crop) / 2)
        # print(crop_mult)
        img_rotated = crop_image(img_rotated, x0, y0, w_crop, h_crop)
    return img_rotated
